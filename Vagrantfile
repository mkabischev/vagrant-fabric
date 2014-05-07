# -*- mode: ruby -*-
# vi: set ft=ruby :

# Assumes a box from https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box

# This sets up 4 nodes, one with fabric-store and 3 nodes.


fabric_nodes = {
  'store' => {
    'ip' => '192.168.70.100',
    'playbook' => 'fabric-store.yml'
  },
  'node1' => {
    'ip' => '192.168.70.101',
    'playbook' => 'fabric-node.yml'
  },
  'node2' => {
    'ip' => '192.168.70.102',
    'playbook' => 'fabric-node.yml'
  },
  'node3' => {
    'ip' => '192.168.70.103',
    'playbook' => 'fabric-node.yml'
  },
}


Vagrant.configure("2") do |config|
  config.vm.box = 'centos65-x86_64-20140116'
  config.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box'

  # Create all three nodes identically except for name and ip
  fabric_nodes.each_pair { |node, node_params|
    config.vm.define node do |node_config|
      node_config.vm.hostname = "#{node}.local"
      node_config.vm.network :private_network, ip: node_params['ip']
      node_config.vm.provision :ansible do |ansible|
        ansible.verbose = ''
        ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
        ansible.playbook = 'provisioning/' + node_params['playbook']
        ansible.host_key_checking = false
        # Disable default limit (required with Vagrant 1.5+)
        ansible.limit = 'all'
        
      end
    end
  }
end
