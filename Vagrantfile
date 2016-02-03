NODE_COUNT = ENV['CASSANDRA_NODES_COUNT'] || 2
NODE_MEMORY = '768'.freeze
CONTROLLER_MEMORY = '128'.freeze

Vagrant.configure('2') do |config|
  # Common box option
  config.vm.box = 'ubuntu/precise64'
  # Nodes defenition
  (1..NODE_COUNT).each do |node_id|
    config.vm.define "cassandra-node-#{node_id}" do |node|
      node.vm.hostname = "cassandra-node-#{node_id}"
      node.vm.network 'private_network', ip: "192.168.56.#{10 + node_id}"
      # Customize memory
      node.vm.provider 'virtualbox' do |vb|
        # vb.gui = true
        vb.customize ['modifyvm', :id, '--memory', NODE_MEMORY]
      end
    end
  end
  # Provisioner
  config.vm.define 'cassandra-controller' do |controller|
    controller.vm.hostname = 'cassandra-controller'
    controller.vm.network 'private_network', ip: '192.168.56.42'
    controller.vm.synced_folder '.', '/vagrant',
                                mount_options: ['dmode=770', 'fmode=600']
    controller.vm.provision 'shell', inline:
        # TODO: Check and eliminate this for Vagrant v.1.8.2 and later
        'sudo apt-get install -q -y python-pip python-dev ' \
        '&& sudo pip install ansible==1.9.2 ' \
        '&& sudo cp /usr/local/bin/ansible /usr/bin/ansible'
    controller.vm.provision :ansible_local do |ansible|
      ansible.install = false
      ansible.limit = 'all'
      # ansible.verbose = true # More information during provision
      ansible.playbook = 'ansible/vagrant.yml'
      ansible.inventory_path = 'ansible/vagrant_inventory'
      # Customize memory
      controller.vm.provider 'virtualbox' do |vb|
        # vb.gui = true
        vb.customize ['modifyvm', :id, '--memory', CONTROLLER_MEMORY]
      end
    end
  end
end
