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

# Nodes:

* SigmaUI does not work with Kibana 7.10 and newer

       link: https://github.com/socprime/SigmaUI


# License

CC-0, WTFPL

No warranty provided. If it breaks, please keep both parts.
