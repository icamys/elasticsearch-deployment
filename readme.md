### Elasticsearch deployment with ansible ###

Playbook for elasticsearch cluster deployment using ansible with ssl transport enabled.

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

1. Credentials are created in new directory called `./credentials` while deploying.

1. Generate ssl certificates.

    Now elasticsearch should start successful but after first restart
    it will fail because x-pack security feature is enabled but ssl transport is not configured
    which is required starting from basic license.

    Note: at the moment the used elasticsearch role does not provide any way to generate 
    and use ssl certificates automatically, that's why we should do in manually.

    See the "Transport TLS/SSL encryption" part in 
    [Elasticsearch Security: Configure TLS/SSL & PKI Authentication](https://www.elastic.co/blog/elasticsearch-security-configure-tls-ssl-pki-authentication)
    for details.

1. Next, you should create local `./certs` directory 
    and copy newly created `elastic-certificates.p12` and `elastic-stack-ca.p12` files into it.

1. After that, stop all your elasticsearch nodes by running:

    Note: check that all_nodes variable is configured properly in `inventory.yml` file. 

        $ ansible-playbook -i inventory.yml pb.es-stop.yml

1. Copy certificates from local directory to all elasticsearch nodes:

        $ ansible-playbook -i inventory.yml pb.es-cp-certs.yml

1. Enable ssl transport:
        
        $ ansible-playbook -i inventory.yml pb.es-enable-ssl.yml

1. Now when the configuration is done, start your elasticsearch nodes. 

    Note: Here could be some transport issues. I've solved them by starting master nodes one by one, 
    and after that I've started all data nodes by manipulating inventory file and running:
    
        $ ansible-playbook -i inventory.yml pb.es-start.yml
    
     
      
