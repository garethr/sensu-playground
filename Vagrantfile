# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "puppet-precise64"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-1204-x64.box"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provision :puppet,
    :options => ["--debug", "--verbose", "--summarize", "--reports", "store"] do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path    = "modules"
    puppet.manifest_file  = "base.pp"
  end

  config.vm.define :server do |conf|
    conf.vm.hostname = 'sensu-server'
    conf.vm.network :private_network, ip: "192.168.50.4"
    conf.vm.network :forwarded_port, guest: 8080, host: 8080
  end

  config.vm.define :client do |conf|
    conf.vm.hostname = 'sensu-client'
    conf.vm.network :private_network, ip: "192.168.50.5"
  end

end
