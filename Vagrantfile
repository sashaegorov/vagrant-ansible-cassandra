NODES = 2
MEMORY = '1024'.freeze

Vagrant.configure("2") do |config|
  (1..NODES).each do |machine_id|
    config.vm.box = 'ubuntu/precise64'
    config.vm.define "node#{machine_id}" do |machine|
      machine.vm.hostname = "node#{machine_id}"
      machine.vm.network 'private_network', ip: "192.168.56.#{10+machine_id}"
      machine.vm.provision :ansible do |ansible|
        ansible.limit = 'all'
        # ansible.verbose = true # More information during provision
        ansible.playbook = 'vagrant.yml'
        config.vm.provider 'virtualbox' do |vb|
          vb.customize ['modifyvm', :id, '--memory', MEMORY]
        end
      end
    end
  end
end
