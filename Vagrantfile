NODE_COUNT = 3
NODE_MEMORY = '1024'.freeze
CONTROLLER_MEMORY = '256'.freeze

puts "Using #{NODE_COUNT} nodes with #{NODE_MEMORY}Mb RAM each."

Vagrant.configure("2") do |config|
  # Common box option
  config.vm.box = 'ubuntu/precise64'
  # Nodes
  (1..NODE_COUNT).each do |node_id|
    config.vm.define "node#{node_id}" do |node|
      node.vm.hostname = "node#{node_id}"
      node.vm.network 'private_network', ip: "192.168.56.#{10+node_id}"
      # Customize memory
      node.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', NODE_MEMORY]
      end
    end
  end
  # Provisioner
  config.vm.define 'controller' do |controller|
    controller.vm.hostname = 'controller'
    controller.vm.network 'private_network', ip: "192.168.56.42"

    controller.vm.provision 'shell',
        # TODO: Check and eliminate this for Vagrant v.1.8.2 and later
        inline: 'sudo apt-get install -q -y python-pip python-dev ' \
        '&& sudo pip install ansible==1.9.2 ' \
        '&& sudo cp /usr/local/bin/ansible /usr/bin/ansible'
    controller.vm.provision :ansible_local do |ansible|
      ansible.install = false
      ansible.limit = 'all'
      ansible.verbose = true # More information during provision
      ansible.playbook = 'ansible/vagrant.yml'
      ansible.inventory_path = 'ansible/vagrant_inventory'
      # Customize memory
      controller.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', CONTROLLER_MEMORY]
      end
    end
  end
end
