# Elasticsearch Vagrant playground

This respository is used as a playground for testing Elasticsearch 
cluters.


## Vagrant test

* Create ES certificate authority
 
      ansible-playbook setup-certs.yml --limit localhost

* Start the machines (requires 8GB of RAM)
 
      vagrant up --no-provision

* Provision the machines  
 
      vagrant provision node1

## Kibana

Connect to http://192.168.49.101:5601

    Login:    testuser  
    Password: nbusr123

Change passwords in `group_vars/vagrant/secrets.yml` 

# Notes:

* SigmaUI does not work with Kibana 7.10 and newer

       link: https://github.com/socprime/SigmaUI


# License

CC-0, WTFPL

Do not use in production.

No warranty provided. If it breaks, please keep both parts.

