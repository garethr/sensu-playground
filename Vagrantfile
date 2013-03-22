# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_1_0 = !Vagrant.respond_to?(:configure)

config_block = Proc.new do |config|
  config.vm.box = "puppet-precise64"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-1204-x64.box"

  if VAGRANT_1_0
    config.vm.customize(["modifyvm", :id, "--memory", "1024"]) 
  else
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end
  end

  config.vm.provision :puppet,
    :options => ["--debug", "--verbose", "--summarize", "--reports", "store"] do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path    = "modules"
    puppet.manifest_file  = "base.pp"
  end

  config.vm.define :server do |conf|
    if VAGRANT_1_0
      conf.vm.host_name = 'sensu-server'
      conf.vm.network :hostonly, "192.168.50.4"
      conf.vm.forward_port 8080, 8080
    else
      conf.vm.hostname = 'sensu-server'
      conf.vm.network :private_network, ip: "192.168.50.4"
      conf.vm.network :forwarded_port, guest: 8080, host: 8080
    end
  end

  config.vm.define :client do |conf|
    if VAGRANT_1_0
      conf.vm.host_name = 'sensu-client'
      conf.vm.network :hostonly, "192.168.50.5"
    else
      conf.vm.hostname = 'sensu-client'
      conf.vm.network :private_network, ip: "192.168.50.5"
    end
  end

end

if VAGRANT_1_0
  Vagrant::Config.run(&config_block)
else
  Vagrant.configure("2", &config_block)
end
