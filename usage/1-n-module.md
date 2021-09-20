# 1:N Face Identification Module

Current module allows to create a database with identities providing various search capabilities.

Main module entities are:

* _**Person**_. Identity with **name**, a set of **Images** and **metadata**. Metadata is a regular json object, specified by end app.
* _**Image**_. Photo of **Person**.
* _**Group**_. A set of **Persons**. You can create any number of **Groups**, add or remove **Persons** from them at any time. By default, **Person** don't belong to any **Group**. Each **Person** can be member of many **Groups**.
* **Storage**. Storage for **Images** of **Person**. Can be multiple storages per application. During **Person**

  creation, you specify which storage to use for saving photos. Thus, you can split for further processing or other needs you photo catalog. Main property of storage is **uri**, allowing to use not only local filesystem, but other remote staff, for example aws s3 buckets etc. \(Currently on filesystem is supported\). Each **Person** can have only one **Storage**.

