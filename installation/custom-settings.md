# Settings

Webservice configures via **env** variables. To avoid names conflict with other tools, use the prefix **FACEAPI\_**. While it is not mandatory, we strongly **recommend** setting the options with prefixes. For readability, we will use the **primary** name for all settings below.

To make configuration a bit easier, use the **.env** file. For all platforms besides docker, the **.env** file is located in an installation root directory. This file is a **text** file containing key-value pairs of all the settings required by your application. Using a **.env** file will enable you to set environment variables for **webservice** without polluting the global environment namespace.

{% hint style="warning" %}
On some systems, files beginning with a dot are hidden by default. Thus, **.env** file can't be seen using standard files viewers or \`ls\` like commands.
{% endhint %}

## General

| Option | Default | Description |
| :--- | :--- | :--- |
| **BIND** | 0.0.0.0:41101 | IpAddress:port server binding |
| **WORKERS** | 1 | Number of workers to process requests |
| **BACKLOG** | WORKERS x 15 | Maximum number of requests in a queue awaiting processing |
| **TIMEOUT** | 30 | Number of seconds for worker to process request. Workers silent for more than this many seconds are killed and restarted. |
| **ENABLE_DEMO_WEB_APP** | "true" | Serve a demo web app under host **root** url (ex. localhost:41101/ )  |

## HTTPS and CORS

{% hint style="warning" %}
 While, HTTPS and CORS can be set directly on webservice, we strongly recommend [running reverse-proxy](./general.md#proxy-guard) in front and move configuration to proxy itself.
{% endhint %}

| Option | Default | Description |
| :--- | :--- | :--- |
| **CORS_ORIGINS** | no default, that means web browser will allow requests to webserver only from a same domain  | origin, allowed to use API |
| **CORS_METHODS** | all methods  | methods, allowed to invoke on API. Specify comma-separated values as single string (ex. "GET,POST,PUT") |
| **CORS_HEADERS** | all headers  | headers, allowed to read from API. Specify comma-separated values as single string (ex. "content-type,date") |

{% hint style="info" %}
See a great [article about CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) from Mozilla
{% endhint %}