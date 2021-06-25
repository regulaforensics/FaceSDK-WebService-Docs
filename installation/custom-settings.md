# Settings

Webservice configures via **env** variables. To avoid names conflict with other tools, use the prefix **FACEAPI\_**. While it is not mandatory, we strongly **recommend** setting the options with prefixes. For readability, we will use the **primary** name for all settings below.

All **relative** paths below is sub-paths for **installation** folder, or **/app/** folder for docker. Let's call it **app root folder**. 

To make configuration a bit easier, use the **.env** file. The **.env** file is located under **app root folder**. This file is a **text** file containing key-value pairs of all the settings required by your application. Using a **.env** file will enable you to set environment variables for **webservice** without polluting the global environment namespace.

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
| **ENABLE\_DEMO\_WEB\_APP** | "true" | Serve a demo web app under host **root** url (ex. localhost:41101/ )  |

## HTTPS and CORS

{% hint style="warning" %}
 While, HTTPS and CORS can be set directly on webservice, we strongly recommend [running reverse-proxy](./general.md#proxy-guard) in front and move configuration to proxy itself.
{% endhint %}

| Option | Default | Description |
| :--- | :--- | :--- |
| **CORS\_ORIGINS** | no default, that means web browser will allow requests to webserver only from a same domain  | origin, allowed to use API |
| **CORS\_METHODS** | all methods  | methods, allowed to invoke on API. Specify comma-separated values as single string (ex. "GET,POST,PUT") |
| **CORS\_HEADERS** | all headers  | headers, allowed to read from API. Specify comma-separated values as single string (ex. "content-type,date") |

{% hint style="info" %}
See a great [article about CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) from Mozilla
{% endhint %}

| Option | Default | Description |
| :--- | :--- | :--- |
| **HTTPS** | "false"  | If enabled, serve webservice via HTTPS using cert and key, specified in options below.  |
| **CERT\_FILE** | "certs/cert.pem" | Specifies a file containing cert file. |
| **KEY\_FILE** | "certs/key.pem" | Specifies a file containing cert key file. |

{% hint style="warning" %}
Use key file without passphrase. Passphrase causes webserver to crash, or infinity await **stdin**.
{% endhint %}

## Logging

There are **3** log types in our service:

1. access logs - are just standard **HTTP** access logs.
2. application logs  - regular application logs, including errors and debug messages.
3. processing results logs - stores processing **input** and **results** in JSON format. **Space-consuming** option, up to a few tens of Mb per request. **Disabled** by default.

| Option | Default | Description |
| :--- | :--- | :--- |
| **LOGS\_ACCESS\_CONSOLE** | "false" | Controls whether to print access logs to a console. |
| **LOGS\_ACCESS\_FILE** | "false" | Controls whether to save access logs to a file. |
| **LOGS\_ACCESS\_FILE\_PATH** | "logs/access/facesdk-reader-access.log" | Specifies a file to save access logs if **LOGS\_ACCESS\_FILE** enabled. |
|  |  |  |
| **LOGS\_APP\_CONSOLE** | "true" | Controls whether to print application logs to a console. |
| **LOGS\_APP\_FILE** | "false" on Docker **\/** "true" on other installations | Controls whether to save application logs to a file. |
| **LOGS\_APP\_FILE\_PATH** | "logs/app/facesdk-reader-app.log" | Specifies a file to save access logs if **LOGS\_APP\_FILE** enabled. |
|  |  |  |
| **PROCESS\_RESULTS\_LOG\_PATH** | "logs/process" | Specifies a folder to save processing results. Final output is a two files(with **_in** and **_out** suffixes), located in **endpoint/yyyy/mm/dd/hh** folder under specified in this property path. |
|  |  |  |
| **LOGS\_FORMATTER** | "text" | Possible values: **"text"** / **"json"**. Some log collectors require logs to be printed in json format. |

Access and applications logs are printed to **stdout**.

For access and applications logs files **day-based** rotation occurs every **midnight UTC**. Service **keeps** the last **30 days** of logs files.

{% hint style="info" %}
On Linux and Win platforms webserver store application logs in a file by default to speedup troubleshooting.
{% endhint %}
