# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

Vagrant.require_version '>= 1.5.0'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  (1..3).each do |i|
    config.vm.define "bitmonk-kafka-berkshelf#{i}" do |s|
      s.vm.hostname = "bitmonk-kafka-berkshelf#{i}"
      s.vm.network "private_network", ip: "10.40.3.#{4-i}0", netmask: "255.255.255.0", virtualbox__intnet: "servidors", drop_nat_interface_default_route: true

      # Enabling the Berkshelf plugin. To enable this globally, add this configuration
      # option to your ~/.vagrant.d/Vagrantfile file
      config.berkshelf.enabled = true

      config.vm.provision :chef_solo do |chef|
        chef.json = {
          mysql: {
            server_root_password: 'rootpass',
            server_debian_password: 'debpass',
            server_repl_password: 'replpass'
          }
        }

        chef.run_list = [
          'recipe[bitmonk_kafka::default]'
        ]
      end
    end
  end


  # Set the version of chef to install using the vagrant-omnibus plugin
  # NOTE: You will need to install the vagrant-omnibus plugin:
  #
  #   $ vagrant plugin install vagrant-omnibus
  #
  if Vagrant.has_plugin?("vagrant-omnibus")
    config.omnibus.chef_version = 'latest'
  end

  config.vm.box = 'chef/ubuntu-14.04'


  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

end
