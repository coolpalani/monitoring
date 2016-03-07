# monitoring

Ansible playbook for provisioning a monitoring server


## What it does

it's an ansible playbook that takes docker compatible linux and puts a full monitoring software on top.

First, ansible installs python pip and docker. Then, an icinga2 docker image (https://hub.docker.com/r/jordan/icinga2/) is downloaded and launched. The docker container initializes a fresh icinga2 instance. Then ansible copies over my custom host/service/admin definitions (over-wrinting stock configurations). Finally, the docker container is restarted, with the custom definitions in effect.

## Running procedure

Make any desired changes to the Icinga host/service/user definitions.

    `roles/provision/files/icinga2-config/conf.d/*.conf`

Install ansible dependencies

    ansible-galaxy install -r requirements.yml

Provision/Deploy. This can be run on a VPS in a clean state, or a VPS that has already run this playbook. Either way will work.

    ansible-playbook -i ~/.ansible-inventory ./provisioning/provision



## Icinga2 notes

Validate config file

```
icinga2 daemon -C
```

Show all available services

```
icinga2 object list --type Service
```

## Misc notes

//sudo docker run -d -v "$(pwd)/custom_configs:/etc/shinken/custom_configs" -p 80:80 rohit01/shinken_thruk_graphite
