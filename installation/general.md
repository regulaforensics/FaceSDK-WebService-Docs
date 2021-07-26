# Getting Started

## Worker model

By default, webservice starts with 1 **worker**. Each worker process requests in a **single-threaded** mode, that mean can process only **one** request at a time. So if you submit many requests at once they will queue up and will be processed one by one.

For improved throughput performance you need to launch more workers according to the planned load. Webservice can spawn **multiple** workers under **one** service instance. You may set workers number based on the [hardware limitation](general.md#prerequisites) you have. Also, you can use **N** webserver instances with **multiple** workers under loadbalancer to increase performance even father.

{% hint style="warning" %}
**Each worker** in our license model counts as **instance**. Thus, setup - 3 machines with 2 workers on each - **requires** a license with 6 instances.

For **transaction-based** licenses number of workers is not limited, only amount of **process requests** counts.
{% endhint %}

## Interactions With License Service

Webservice when working with **online** license contacts Licensing Service at [https://lic.regulaforensics.com](https://lic.regulaforensics.com) via **encrypted** communication and sends requests for validation.

{% hint style="info" %}
The Licensing Service **only** serves the purpose of the license keys management. It does not change the fact that the webservice remains customer-hosted, on-prem, and all the personal data remains on the Customer's servers and is not being communicated back to Regula.
{% endhint %}

On the **start** of a worker, licensing service is contacted, and a registration request is sent. If the license is valid, the worker runs \(or fail otherwise\).

In case the online license is transaction-based, **each time** the functionality is used, the webservice contacts the Licensing Service with POST request. This request **consist of**: license id, transaction id, session id, client ip, transaction scenario, product type and version used, timestamp. If the license is **valid** for this kind of request, the request gets logged, the 'OK' response is provided, and the SDK **returns** the processing results.

In case the online license is instance/worker-based, **heartbeats** will be monitored instead: Every **hour** the webservice will send a heartbeat request to the Licensing Service providing the **following** information: license id, session id, client ip, product type and version used, timestamp. If license is valid, and the response is **OK**, the webservice **continues** to perform. Alternatively, if the response is **FAIL**, the Error message will show.

Heartbeats will **continue** and as soon as the License **becomes** valid again, the operation of the webservice will restore **automatically**. In case there is **no** Internet connection when the request is sent, the result will be **TIMEOUT**.

### Connect to license service via HTTP proxy

If you host our solution in an isolated private environment, you can specify HTTP proxy via  `HTTPS_PROXY` env variable. Proxy will be used by webservice to connect to license service.

{% hint style="warning" %}
Do not specify protocol prefix in proxy URL. Instead `HTTPS_PROXY=http(s)://host:port` use `HTTPS_PROXY=host:port`. 
{% endhint %}

{% hint style="info" %}
If you use your own tls certs, place them in `/etc/ssl/certs` folder in linux and docker envs.
{% endhint %}

## Proxy Guard

**Regula Face SDK** is **not** general HTTP webserver, that handles hundreds of request per second. Typical processing takes up to a few seconds, so we can call it CPU intensive. Considering that typical instance has 1-4 workers, we need to carefully manage workers time. One of the main sources of **waisting** worker's processing time is **slow clients** problem. When webservice receives request from client with a **slow** internet connection, free worker start receiving that request. The worker will be bottlenecked by the speed of the client connection, and it will be blocked **until** the slow client finishes sending **large** ID image. Being blocked means that this worker process can **not** handle any other request in the meantime, it’s just there, **idle**, waiting to receive the entire request, so it can start really processing it.

We strongly recommend using **Regula Face SDK** behind a proxy server. Although there are many HTTP proxies available, we strongly advise that you use **Nginx**. If you choose another proxy server, you need to make sure that it buffers slow clients.

{% hint style="danger" %}
Typical Load Balancers from cloud provides, such as ELB/ALB from aws, do **not** buffer slow clients. So you **still need** to use some **proxy** server between LB and **Regula Face SDK**.
{% endhint %}

## Prerequisites

Recommended machine settings: 1CPU and 3Gb RAM per worker.

{% hint style="warning" %}
For using docker / Linux instances you will need an **Online** license. Internet connection is a mandatory requirement for an **Online** license, thus the instance with pre-installed service must have internet access at least to [**https://lic.regulaforensics.com/**](https://lic.regulaforensics.com/) URL.
{% endhint %}

## Versioning

The Version `A.B.C.D` is composed of:

* Regula SDK version `A.B`
* Neural network version `C`
* Service wrapper version `D`

An example of version **3.0.310.2** can be interpreted as:

* the Regula SDK version is `3.0`
* the current neural network version is `310`
* the webservice wrapper version is `2`

