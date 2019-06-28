### Elasticsearch deployment with ansible ###

Deploy elasticsearch cluster using ansible.

Tested on ansible 2.8.1, python version = 3.6.8

Deployment steps:

1. Install required roles:

        $ ansible-galaxy install -r requirements.yml

1. Copy and change `inventory.sample.yml` file:

        $ cp inventory.sample.yml inventory.yml

1. Deploy elasticsearch master servers:

        $ ansible-playbook -i inventory.yml pb.es-master.yml

1. Deploy elasticsearch data servers:

        $ ansible-playbook -i inventory.yml pb.es-data.yml
