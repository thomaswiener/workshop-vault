## General

Create a new vault user by logging in to target database

```
 mysql> CREATE USER 'vault'@'%' IDENTIFIED BY 'password';
 mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'vault'@'%' WITH GRANT OPTION;
 ```

## Create initial db connection config
```
$ vault write database/config/sfirm \
    plugin_name=mysql-database-plugin \
    connection_url="{{username}}:{{password}}@tcp(name.cluster-xxxxxxxxxx.eu-central-1.rds.amazonaws.com:3306)/" \
    allowed_roles="sfirm-read-only" \
    username="vault" \
    password="password"
```

## Rotate password
```
$ vault write -force database/rotate-root/sfirm
```

## Setup a role

```
$ vault write database/roles/sfirm-read-only \
    db_name=sfirm \
    creation_statements="CREATE USER '{{name}}'@'%' IDENTIFIED BY '{{password}}';GRANT SELECT ON *.* TO '{{name}}'@'%';" \
    default_ttl="1h" \
    max_ttl="24h"

```

## Request credentials (CLI)

```
$ vault read database/creds/sfirm-read-only

# Key                Value
# ---                -----
# lease_id           database/creds/read-only/kIvTZW7rFupUSDxxxxxxx
# lease_duration     1h
# lease_renewable    true
# password           A1a-tPJpf6xxxxxxxxx
# username           v-root-read-only-xxxxxxxxxxxxxx

# login to mysql with given username/password => expect: success
```

## Revoke credentials (CLI)
```
# revoke
# vault lease revoke {lease_id}
vault lease revoke database/creds/read-only/kIvTZW7rFupUSDxxxxxxx
# try relogin  => expect: access denied
```

## Request credentials (API)

```
# curl \
#     -H "X-Vault-Token: {token}" \
#     -X GET \
#     http://127.0.0.1:8099/v1/database/creds/sfirm-read-only
    
curl \
    -H "X-Vault-Token: s.R4NxiZYIwQ6IPAxxxxxxx" \
    -X GET \
    http://127.0.0.1:8099/v1/database/creds/sfirm-read-only
    
 {
   "request_id": "5da46d9f-f6a6-15e8-a936-d2af31bb54c9",
   "lease_id": "database/creds/sfirm-read-only/1hd5M0eCwpdulxxxxxxx",
   "renewable": true,
   "lease_duration": 3600,
   "data": {
     "password": "A1a-nJitiqcWCrDf6gmu",
     "username": "v-token-sfirm-read-xxxxxxxxxx"
   },
   "wrap_info": null,
   "warnings": null,
   "auth": null
 }
```

## Revoke credentials (API)
```
curl \
    -H "X-Vault-Token: s.R4NxiZYIwQ6IPAxxxxxxx" \
    -X PUT \
    --data '{"lease_id": "database/creds/sfirm-read-only/1hd5M0eCwpdulxxxxxxx"}' \
    http://127.0.0.1:8099/v1/sys/leases/revoke
```


## Resources
- https://www.vaultproject.io/api-docs
- https://www.vaultproject.io/docs
- https://www.vaultproject.io/docs/secrets/databases
