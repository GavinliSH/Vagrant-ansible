CONFIGURATIONS = YAML.load_file('nodes.yaml')

Vagrant.configure("2") do |config|
  # changing default timeout from 300 to 1000 seconds
  config.vm.boot_timeout = 1000
  # going through each server configuratioin specification
  CONFIGURATIONS['boxes'].each do |box|
    # define the vm configuration
    config.vm.define box['output_box'] do |server_config|
      # define the base vagrant box to be used
      server_config.vm.box = box['base_box']
      # define the host name
      server_config.vm.host_name = box['output_box']

      # Diasbling the synched folder
      server_config.vm.synced_folder ".", "/vagrant", disabled: true

      # set the network configurations for the vm
      server_config.vm.network :private_network,ip: box['ip']

      memory = 2048
      cpu = 1
      
      server_config.vm.provider :virtualbox do |vb|
        vb.name = box['output_box_name']
        vb.check_guest_additions = true
        vb.functional_vboxsf = false
        vb.gui = false
        vb.customize ['modifyvm', :id, '--memory', memory]
        vb.customize ['modifyvm', :id, '--cpus', cpu]
      end
      # run ansible   
      config.vm.provision "ansible" do |ansible|
        ansible.inventory_path = "inventories/hosts"
        #ansible.limit = box['output_box_name']
        ansible.host_key_checking = false
        ansible.playbook = box['ansible_playbook']
      end
      
    end
  end
end