# Settings

Webservice configures via **env** variables. To avoid names conflict with other tools, use the prefix **FACEAPI\_**. While it is not mandatory, we strongly **recommend** setting the options with prefixes. For readability, we will use the **primary** name for all settings below.

To make configuration a bit easier, use the **.env** file. For all platforms besides docker, the **.env** file is located in an installation root directory. This file is a **text** file containing key-value pairs of all the settings required by your application. Using a **.env** file will enable you to set environment variables for **webservice** without polluting the global environment namespace.

{% hint style="warning" %}
On some systems, files beginning with a dot are hidden by default. Thus **.env** file can't be seen using standard files viewers or \`ls\` like commands.
{% endhint %}

