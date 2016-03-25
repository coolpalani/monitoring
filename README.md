# monitoring

Ansible playbook for provisioning a monitoring server

![A picture of two dudes in a dark room with a dozen LCD displays monitoring cameras and equipment in a water treatment facility. Bad ass monitoring tools. source: http://scs.com/wp-content/uploads/2013/09/UWCORP_Water_Treatment_Plant_Monitoring.jpg](/monitoring.jpg?raw=true "Monitor all the things!")

## What it do

it's an ansible playbook that takes docker compatible linux and puts a full monitoring software on top.

First, ansible installs python pip and docker. Then, an icinga2 docker image (https://hub.docker.com/r/jordan/icinga2/) is downloaded and launched. The docker container initializes a fresh icinga2 instance. Then ansible copies over my custom host/service/admin definitions (over-wrinting stock configurations). Finally, the docker container is restarted, with the custom definitions in effect.


## Running procedure

Make any desired changes to the Nagios host/service/user definitions

    `roles/provision/files/icinga2-config/conf.d/*.conf`

Install ansible dependencies

    ansible-galaxy install -r requirements.yml --roles-path ./roles

Provision/Deploy. This can be run on a VPS in a clean state, or a VPS that has already run this playbook. Either way will work. In other words, run this to get started, and every time you make changes to the nagios host/service/user definitions in this repository. Note: you should not make changes directly on the VPS. Always make changes in this repository, commiting any changes.

    ansible-playbook -i ~/.ansible-inventory --vault-password-file ~/.ansible-vault-password ./main.yml



## Troubleshooting

at one point, I was having difficulty getting postfix to relay mail to mailgun and then my email. I was getting this error in mailgun logs:

```
Failed: root@e5c68db50333 → xtoast@gmail.com 'hello friend' Server response: 550 550 5.7.1 [184.173.153.206 11] Our system has detected that this message is 5.7.1 not RFC 5322 compliant. To reduce the amount of spam sent to Gmail, 5.7.1 this message has been blocked. Please review 5.7.1 RFC 5322 specifications for more information. e136si4210349qhc.97 - gsmtp
```

I couldn't find exact 5.7.1 in RFC 5322, but section 5. is about formatting and whitespace. Checking mailgun logs, I saw that the message From: field had escaped quotes, `\"root\"` so what I did was disable user's ability to set their own From: address in the ssmtp config inside the docker container. `FromLineOverride=NO` in /etc/ssmtp/ssmtp.conf https://github.com/insanity54/monitoring/blob/fc6eb9eaf1dff508f2a083213313764232a7109c/roles/provision/tasks/postfix.yml

## Misc notes


## Contributing

This isn't really an open source project. It's an open-source file-defined infrastructure. Copy and enjoy!

