# -*- mode: ruby -*-
# vi: set ft=ruby :
#
$ansible_mode = 'ansible' or 'ansible_local'

VAGRANTFILE_API_VERSION = "2"

VIRTUAL_MACHINES = {
  :node01 => {
    :vagrant_private_ip             => '192.168.49.101',
    :first          => true
  },
  :node02 => {
    :vagrant_private_ip             => '192.168.49.102',
  },
  :node03 => {
    :vagrant_private_ip             => '192.168.49.103',
  },
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/debian-10"
  config.vm.box_check_update = false

  config.ssh.insert_key = false

  VIRTUAL_MACHINES.each do |name,cfg|

        config.vm.define name do |vm_config|
              vm_config.vm.hostname = name
              vm_config.vm.network :private_network, ip: cfg[:vagrant_private_ip]
              #vm_config.vm.synced_folder ".", "/vagrant", type: "nfs"
              vm_config.vm.synced_folder ".", "/vagrant", enabled: false
              # check https://www.vagrantup.com/docs/synced-folders/nfs.html

              config.vm.provider :virtualbox do |vb|
                vb.memory = 4096
                vb.cpus = 2
                vb.linked_clone = true
              end # provider

              # prepare certificates
              if VIRTUAL_MACHINES[name][:first]
                  config.vm.provision $ansible_mode do |ansible|
                        ansible.playbook = "setup-certs.yml"
                        ansible.become_user = 'root'
                        ansible.become = true
                        #if ENV['ANSIBLE_TAGS'] then ansible.tags = ENV['ANSIBLE_TAGS']; end
                        ansible.groups = {
                          'vagrant'     => VIRTUAL_MACHINES.keys,
                          'elasticsearch_nodes'  => [ 'node01', 'node02', 'node03' ],
                        }
                        ansible.host_vars = VIRTUAL_MACHINES
                        ansible.limit = 'all'
                  end
              end

              if VIRTUAL_MACHINES[name][:first]
                  config.vm.provision $ansible_mode do |ansible|
                        ansible.playbook = "setup-elastic-stack.yml"
                        ansible.become_user = 'root'
                        ansible.become = true
                        if ENV['ANSIBLE_TAGS'] then ansible.tags = ENV['ANSIBLE_TAGS']; end
                        ansible.groups = {
                          'vagrant'     => [ 'node01', 'node02', 'node03' ],
                          'elasticsearch_nodes'  => [ 'node01', 'node02', 'node03' ],
                          'client_nodes' => [ 'node01', 'node02', 'node03' ],
                          'kibana_nodes' => [ 'node01' ],
                        }
                        ansible.host_vars = VIRTUAL_MACHINES
                        ansible.limit = 'all'
                  end
              end
        end
  end
end
