## General

Is a self-hosted service that can do all that. It offers:

Secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.

Addtionally all accesses are limited with a TTL by default.

### Workshop Goals

- Setup Vault Server
- Configure Vault to create DB User
- Call API to consume the requested resources
- Configure Vault to create User Account (Linux)

### Achievements

- [x] Setup Vault Server
- [x] Configure Vault to create DB User
- [ ] Call API to consume the requested resources
- [x] Configure Vault to create User Account (Linux)

### Additional

#### Images

- Ansible scripts and Packer AWS AMI implementation for Vault and Vault-Agent
- How to build packer images

```
cd packer/templates
#update subnet_id in yaml files first for your environment, then
packer build vault.yaml
packer build vault-agent.yaml
```
- Boot up EC2 instances from ami images

#### Documentation about database engine

see [./engines/databases/mysql/sfirm.md](./engines/databases/mysql/sfirm.md)