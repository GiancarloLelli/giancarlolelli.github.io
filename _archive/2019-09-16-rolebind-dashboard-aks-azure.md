---
layout: post
title:  "Risolvere il problema dei permessi sulla Dashboard di AKS"
date:   2019-09-16 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Nel mio ultimo progetto su AKS mi sono trovato ad affrontare il problema di non avere abbastanza permessi sulla mia utenza di sistema per visualizzare tutti gli oggetti all'interno del mio cluster AKS tramite la dashboard che si apre con il comando *kubectl proxy* o con il comando *aks browse* suggerito dal portale di Azure. Dopo un pò di Googlate in giro ho risolto eseguendo applicand oil seguente file yaml tramite la CLI.
```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kube-system
```
