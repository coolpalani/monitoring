# monitoring

Ansible playbook for provisioning a monitoring server


## Satisfy dependencies

    ansible-galaxy install -r requirements.yml

## Provision/Deploy

    ansible-playbook -i ~/.ansible-inventory ./provisioning/provision
    
    


## Misc notes

//sudo docker run -d -v "$(pwd)/custom_configs:/etc/shinken/custom_configs" -p 80:80 rohit01/shinken_thruk_graphite
    