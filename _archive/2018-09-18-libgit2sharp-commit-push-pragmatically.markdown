---
layout: post
title:  "Utilizzare LibGit2Sharp per integrarsi con Git da codice"
date:   2018-09-18 9:00:00 +0200
categories: development
---
{: style="text-align: justify"}
Durante lo sviluppo dell'[Avanade Dynamics 365 Toolkit](https://marketplace.visualstudio.com/items?itemName=GiancarloLelli.AvanadeCRMToolkit) ho dovuto migrare l'integrazione con TFSVC a Git in modo da allineare tutto il prodotto a quello che sta ormai diventando lo standard anche in mondo Microsoft. Per interagire con Git da codice però, non è necessario utilizzare le API di VSTS - bellissimo, dato che tirano giù una marea di dipendenze - ma è sufficente parlare con il "Git installato in locale". Per fare tutto da C# esiste una libreria semplicissima da usare che si chiama [libgit2sharp](https://github.com/libgit2/libgit2sharp).  
  
{: style="text-align: justify"}
Ecco in basso alcuni snippet (presi direttamente dal [VersioningService](https://github.com/GiancarloLelli/xrmdeployvsx/blob/master/src/CRMDevLabs.Toolkit.AzOps/VersioningService.cs) del Toolkit) per la discovery dei file che hanno subito delle change:
```csharp
public SourceControlResultModel QueryLocalRepository()
{
    SourceControlResultModel result = new SourceControlResultModel();

    try
    {
        using (var gitRepo = new Repository(m_repoPath))
        {
            m_progress?.Invoke("------------------- START -----------------------");
            m_progress?.Invoke($"[AZOPS] => Detecting changes in the workspace.");

            var repoStatus = gitRepo.RetrieveStatus();
            var totalChanges = repoStatus.Count();
            var pendings = new List<RawChanges>();

            m_progress?.Invoke($"[AZOPS] => Found {repoStatus.Added.Count()} added items.");
            m_progress?.Invoke($"[AZOPS] => Found {repoStatus.Modified.Count()} modified items.");
            m_progress?.Invoke($"[AZOPS] => Found {repoStatus.Removed.Count()} removed items.");

            for (int i = 0; i < totalChanges; i++)
            {
                var item = repoStatus.ElementAt(i);

                if (item.State != FileStatus.DeletedFromWorkdir && item.State != FileStatus.ModifiedInWorkdir && item.State != FileStatus.NewInWorkdir)
                    continue;

                // Path normalization
                var fixedPath = item.FilePath.Replace('/', '\\');
                var firstSlashIndex = fixedPath.IndexOf('\\');
                fixedPath = fixedPath.Substring(firstSlashIndex);

                pendings.Add(new RawChanges
                {
                    ChangeTypeName = item.State.ToString(),
                    LocalItem = string.Concat(m_basePath, fixedPath),
                    FileName = new FileInfo(item.FilePath).Name
                });
            }

            result.Changes = pendings.ToArray();
        }
    }
    catch (Exception ex)
    {
        m_progress?.Invoke($"[ERROR] => {ex.Message}");
        m_telemetry.TrackExceptionWithCustomMetrics(ex);
        result.Continue = false;
    }

    return result;
}
```
E successivamente per fare il commit e successivo push
```csharp
public void CommitAndPush(string password)
{
    try
    {
        using (var gitRepo = new Repository(m_repoPath))
        {
            var status = gitRepo.RetrieveStatus();

            // Meaningful pending changes
            var editPaths = status.Modified.Select(mods => mods.FilePath).ToList();
            var addPath = status.Added.Select(mods => mods.FilePath).ToList();
            var deletePath = status.Removed.Select(mods => mods.FilePath).ToList();

            var allChanges = new List<string>();
            allChanges.AddRange(editPaths);
            allChanges.AddRange(addPath);
            allChanges.AddRange(deletePath);

            if (allChanges.Count > 0)
            {
                // Stage, Commit & Push
                Commands.Stage(gitRepo, allChanges);
                var agent = new Signature("Author Name", "suppert@email.com", DateTimeOffset.Now);
                gitRepo.Commit($"CI commit by {Environment.UserName}", agent, agent);

                var remote = gitRepo.Network.Remotes["origin"];
                if (remote == null)
                    throw new Exception("Unable to find 'origin' in remote branches list");

                gitRepo.Network.Push(remote, m_branch, new PushOptions { CredentialsProvider = (a, b, supported) => new UsernamePasswordCredentials { Username = m_username, Password = password } });
            }
        }
    }
    catch (Exception ex)
    {
        m_progress?.Invoke($"[ERROR] => {ex.Message}");
        m_telemetry.TrackExceptionWithCustomMetrics(ex);
    }
}
```
{: style="text-align: justify"}
Molto interessante è il fatto che quando istanziamo l'oggetto **PushOptions** per far si che la push su Azure DevOps vada a buon fine è necessario passare come Username una stringa contentente un *whitespace* mentre come password un *Personal Access Token* creato direttamente dal portale della secuirty di Azure Dev Ops.