# Settings

Webservice is configured via **env** variables. To avoid names conflict with other tools, use the prefix **FACEAPI\_**. While it is not mandatory, we strongly **recommend** setting the options with prefixes. For readability, we will use the **primary** name for all settings below.

All **relative** paths below are sub-paths for an **installation** folder, or **/app/** folder for docker. Let's call it **app root folder**.

To make configuration a bit easier, use the **.env** file. The **.env** file is located under **app root folder**. This file is a **text** file containing key-value pairs of all the settings required by your application. Using a **.env** file will enable you to set environment variables for **webservice** without polluting the global environment namespace.

{% hint style="warning" %}
On some systems, files beginning with a dot are hidden by default. Thus, **.env** file can't be seen using standard file viewers or \`ls\` like commands.
{% endhint %}

## General

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Default</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>BIND</b>
      </td>
      <td style="text-align:left">0.0.0.0:41101</td>
      <td style="text-align:left">IpAddress:port server binding</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>WORKERS</b>
      </td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">Number of workers to process requests</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BACKLOG</b>
      </td>
      <td style="text-align:left">WORKERS x 15</td>
      <td style="text-align:left">Maximum number of requests in a queue awaiting processing</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>TIMEOUT</b>
      </td>
      <td style="text-align:left">30</td>
      <td style="text-align:left">Number of seconds for a worker to process a request. Workers silent for
        more than this many seconds are killed and restarted.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ENABLE_DEMO_WEB_APP</b>
      </td>
      <td style="text-align:left">&quot;true&quot;</td>
      <td style="text-align:left">Serve a demo web app under host <b>root</b> url (ex. localhost:41101/ )</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>LIC_URL</b> [docker only]</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">URL to regula.license file for further download, if the mount option is
        not available</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>HTTPS_PROXY</b>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>HTTP proxy, used to connect to license service. Do not specify protocol
          prefix in proxy URL.
          <br />Instead of <code>HTTPS_PROXY=http(s)://host:port</code> use <code>HTTPS_PROXY=host:port</code>
        </p>
        <p>If you use your own TSL certs, place them in <code>/etc/ssl/certs</code> folder
          in Linux and docker envs.</p>
      </td>
    </tr>
  </tbody>
</table>

## HTTPS and CORS

{% hint style="warning" %}
While HTTPS and CORS can be set directly on a webservice, we strongly recommend [running reverse-proxy](general.md#proxy-guard) in front and move a configuration to a proxy itself.
{% endhint %}

| Option | Default | Description |
| :--- | :--- | :--- |
| **CORS\_ORIGINS** | no default, that means web browser will allow requests to webserver only from the same domain | origin, allowed to use API |
| **CORS\_METHODS** | all methods | methods, allowed to invoke on API. Specify comma-separated values as single string \(ex. "GET,POST,PUT"\) |
| **CORS\_HEADERS** | all headers | headers, allowed to read from API. Specify comma-separated values as single string \(ex. "content-type,date"\) |

{% hint style="info" %}
See a great [article about CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) from Mozilla
{% endhint %}

| Option | Default | Description |
| :--- | :--- | :--- |
| **HTTPS** | "false" | If enabled, serve webservice via HTTPS using cert and key, specified in options below. |
| **CERT\_FILE** | "certs/cert.pem" | Specifies a file containing cert file. |
| **KEY\_FILE** | "certs/key.pem" | Specifies a file containing cert key file. |

{% hint style="warning" %}
Use key file without a passphrase. Passphrase causes webserver to crash, or infinity awaits **stdin**.
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
| **PROCESS\_RESULTS\_LOG\_PATH** | "logs/process" | Specifies a folder to save processing results. The final output is two files\(with **\_in** and **\_out** suffixes\), located in **endpoint/yyyy/mm/dd/hh** folder under specified in this property path. |
|  |  |  |
| **LOGS\_LEVEL** | "info" | Specify application logs level. Possible values: "error", "warn", "info", "debug" |
| **LOGS\_FORMATTER** | "text" | Possible values: **"text"** / **"json"**. Some log collectors require logs to be printed in json format. |

Access and applications logs are printed to **stdout**.

For access and applications logs files **day-based** rotation occurs every **midnight UTC**. Service **keeps** the last **30 days** of log files.

{% hint style="info" %}
On Linux and Win platforms webserver stores application logs in a file by default to speed up troubleshooting.
{% endhint %}

