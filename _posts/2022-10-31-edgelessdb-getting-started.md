---
layout: post
title:  "EdgelessDB Getting Started"
date:   2022-10-31 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Dopo il primo post su EdgelessDB ho avuto finalmente modo di provarlo (in modalità simulazione) sul mio Laptop di lavoro, devo dire che l'esperienza è stata seamless e per nulla complicata nonostante il "regno" in cui questa tecnologia si colloca. Gli step fondamentali per avviare un istanza di EdgelessDB sono:

1. Avvio del container tramite Docker (rif comando nel post di introduzione a EdgelessDB)
2. Generazione dei certificati e del manifest di boostratp dell'istanza del DBMS
3. Verifica dell'istanza di EdgelessDB tramite Remote Attestation
4. Applicazione del manifest creato allo step #2
5. Utilizzo di EdgelessDB

Molto interessante come EdgelessDB utilizzi un suo servizio di verifica del TEE denominato **Edgeless Remote Attestation** o più comunemente detto `era` tool

```powershell
wget https://github.com/edgelesssys/edgelessdb/releases/latest/download/edgelessdb-sgx.json
era -c edgelessdb-sgx.json -h localhost:8080 -output-root edb.pem
```

Il link al Getting Started completo è [questo](https://docs.edgeless.systems/edgelessdb/getting-started/quickstart-sgx).