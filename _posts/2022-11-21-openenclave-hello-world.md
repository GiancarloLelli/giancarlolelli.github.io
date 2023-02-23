---
layout: post
title:  "Hello World di Open Enclave"
date:   2022-11-21 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Il sample Hello World del repository di Open Enclave devo dire che è si complicato, ma abbastanza fatto bene per quanto riguarda la dimostrazione delle capability necessaria in un contesto basico - ovvero, "devo creare un enclave senza troppi fronzoli, come faccio"? Di base infatto il sample fa vedere (cito direttamente in inglese)

* The host creates an enclave
* The host calls a simple function in the enclave
* The enclave function prints a message and then calls a simple function back in the host
* The host function prints a message before returning to the enclave
* The enclave function returns back to the host
* The enclave is terminated

La cosa itneressante è questa nuova tipologia di file `edl` che è necessario conoscere, scrivere e mantenere per poter andare a generare tutto il codice di mashalling che poi l'applicativo host utilizzerà.

```plaintext
enclave {
    trusted {
        public void enclave_helloworld();

    };

    untrusted {
        void host_helloworld();
    };
};
```
l'SDK di OpenEnclave utilizza un tool denominato `oeedger8r` per la creazione del codice di marshalling necessario.

> To generate the functions with the marshaling code the oeedger8r is called in both the host and enclave directories. To generate the marshaling code the untrusted host uses to call into the trusted enclave, the --untrusted argument. To generate the marshaling code the trusted enclave uses to call into the untrusted host, the --trusted argument. oeedger8r needs the search path as an input to figure out where to search for the edl files.

```plaintext
> oeedger8r --search-path /opt/openenclave/include --search-path /opt/openenclave/include/openenclave/edl/sgx ../helloworld.edl --untrusted
> oeedger8r --search-path /opt/openenclave/include --search-path /opt/openenclave/include/openenclave/edl/sgx ../helloworld.edl --trusted
```

Lascio qui il [link](https://github.com/openenclave/openenclave/blob/master/samples/helloworld/README.md#host-application) della sezione **Host** della guida perchè molto articolata e interessante.