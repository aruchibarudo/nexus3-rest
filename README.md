nexus3-rest
=========

Control nexus3-oss by REST API. Doesn't work yum and raw repos - REST API limitation

Requirements
------------

Installed nexus3 repository manager. YAML file with roles.

Role Variables
--------------
```yml
nexus_server: localhost # nexus server
nexus_schema: http # http/https
nexus_port: 8080
nexus_context: /
nexus_base_url: service/rest # base url for api
DEBUG: [1:3] # debug variables

repo: # repo.yml
  name: "docker-hub-external-dso"
  type: "proxy"
  format: "docker"
  online: false
  proxy:
    remoteUrl: https://registry-1.docker.io
  docker:
    v1Enabled: true
  dockerProxy:
    indexType: HUB

roles: # roles.yml
  Nexus_Base_Role:
    description: "Базвовая роль нексус"
    source: default
    privileges:
    - nx-repository-view-{tech}-{repo}-browse # {tech} -> repo.format, {repo} -> repo.name
    - nx-search-read
  Developer:
    description: "Разработчик"
    source: default
    only: whitelist # will be used if repo.name contain this
    privileges:
    - nx-repository-view-{tech}-{repo}-browse
    - nx-repository-view-{tech}-{repo}-read
    - nx-component-upload
    - nx-search-read
    - nx-apikey-all
```

Dependencies
------------

nope

Example Playbook
----------------

```yaml
    - hosts: nexus
      vars:
        nexus_server: localhost # nexus server
        nexus_schema: http # http/https
        nexus_port: 8080
        nexus_user: "admin"
        nexus_password: "changeme"

      roles:
        - nexus3-rest
```

```bash
# 
$ ansible-playbook nexus.yml -i nexus.ini --extra-vars=@roles.yml --extra-vars=@repo.yml
```

Author Information
------------------

Artem Batalov <archyz@gmail.com>
