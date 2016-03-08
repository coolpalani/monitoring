# monitoring

Ansible playbook for provisioning a monitoring server

![A picture of two dudes in a dark room with a dozen LCD displays monitoring cameras and equipment in a water treatment facility. Bad ass monitoring tools.](/monitoring.jpg?raw=true "Monitor all the things!")

## What it do

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

## Troubleshooting

at one point, I was having difficulty getting postfix to relay mail to mailgun and then my email. I was getting this error in mailgun logs:

```
Failed: root@e5c68db50333 → xtoast@gmail.com 'hello friend' Server response: 550 550 5.7.1 [184.173.153.206 11] Our system has detected that this message is 5.7.1 not RFC 5322 compliant. To reduce the amount of spam sent to Gmail, 5.7.1 this message has been blocked. Please review 5.7.1 RFC 5322 specifications for more information. e136si4210349qhc.97 - gsmtp
```

I couldn't find exact 5.7.1 in RFC 5322, but section 5. is about formatting and whitespace. Checking mailgun logs, I saw that the message From: field had escaped quotes, `\"root\"` so what I did was disable user's ability to set their own From: address in the ssmtp config inside the docker container. `FromLineOverride=NO` in /etc/ssmtp/ssmtp.conf https://github.com/insanity54/monitoring/blob/fc6eb9eaf1dff508f2a083213313764232a7109c/roles/provision/tasks/postfix.yml

## Misc notes

//sudo docker run -d -v "$(pwd)/custom_configs:/etc/shinken/custom_configs" -p 80:80 rohit01/shinken_thruk_graphite
