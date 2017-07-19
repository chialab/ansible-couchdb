CouchDB
=======

This role's duty is to set up a [CouchDB Server](http://couchdb.apache.org/), configure it at wish, and add a basic initialization (create users, create and secure databases).

Variables
---------

### `couchdb_settings`
CouchDB configuration settings, to be written in an `.ini` file within the `/etc/couchdb/local.d/` directory. This is a two-dimensional dict of settings, where the first level keys represent the section.

**Note**: boolean values should be written as strings (e.g.: `"true"`) to prevent unwanted capitalization.

### `couchdb_admins`
Dictionary of CouchDB server admins and their passwords. Passwords **MUST** be written in plain text (please, consider using [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html)). By default, [no admin user is created](http://docs.couchdb.org/en/1.6.1/intro/security.html#the-admin-party).

### `couchdb_users`
Dictionary of CouchDB users to be created. Each dictionary item is itself a dictionary, where the `password` key is required, and the `roles` key (list) is optional. Please note that existing users are **NOT** updated. By default, no user is created.

### `couchdb_databases`
Dictionary of CouchDB databases to be created, and their security settings. Each dictionary item is itself a dictionary, where the `admins` and `members` keys are both optional (see [CouchDB docs](http://docs.couchdb.org/en/1.6.1/api/database/security.html) for reference). By default, no database is created.

### `couchdb_replication`
List of replication settings to be written to `_replicator` database to set up [replication](http://docs.couchdb.org/en/1.6.1/api/server/common.html#replicate). By default, no replication happens.

### `couchdb_url`
Endpoint for HTTP API requests. You should change this only if CouchDB is listening on a non-standard port or on other particular conditions. If you aren't initializing your database with regular users and/or databases, this setting is completely ignored.

Example
-------

```yaml
---
couchdb_settings:
  httpd:
    port: "5984"
    bind_address: "0.0.0.0"

couchdb_admins:
  admin: P4s$w0rd

couchdb_users:
  user_with_roles:
    password: password123
    roles:
      - developer
      - manager
  user_without_roles:
    password: password456

couchdb_databases:
  database_without_security: {}
  database_with_admins:
    admins:
      names:
        - user_without_roles
      roles:
        - manager
  database_with_members:
    members:
      roles:
        - developer

couchdb_replication:
  - source: my_database
    target: http://backup.example.com:5984/my_database
    continuous: yes

couchdb_url: "http://localhost:5984"
```

Contributing
------------

Issues and pull requests are more than welcome!

This repo is a split of the main code that can be found [here](https://github.com/Chialab/ansible-roles).
Please, open pull requests against that repository instead.
