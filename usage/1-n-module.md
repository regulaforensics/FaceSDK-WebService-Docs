# 1:N Face Identification Module

Current module allows to create a database with identities providing various search capabilities.

Main module entities are:

- **Person**. Identity with __name__, a set of **Images** and __metadata__. Metadata is a regular json object, specified
  by end app.
- **Image**. Photo of **Person**.
- **Group**. A set of **Persons**. You can create any number of **Groups**, add or remove **Persons** from them at any
  time. By default, **Person** don't belong to any **Group**. Each **Person** can be member of many **Groups**.
  **Groups** allow limiting identification scope.
- **Storage**. Storage for **Images** of **Person**. Can be multiple storages per application. During **Person**
  creation, you specify which storage to use for saving photos. Thus, you can split for further processing or other
  needs you photo catalog. Main property of storage is __uri__, allowing to use not only local filesystem, but other
  remote staff, for example aws s3 buckets etc. (Currently on filesystem is supported). Each **Person** can have only
  one **Storage**.
