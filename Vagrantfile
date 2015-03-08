# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

AGENT_BOX_MEM = "1024"
BOX_URI = "box-cutter/oel66"
SERVER_BOX_MEM = "2048"

HOSTS_NO_PROXY = "localhost,127.0.0.0/8,puppetagent01,puppetagent02,puppetmaster"
SOURCE_DIR = "/vagrant/src"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box_download_insecure = true
  config.proxy.no_proxy = HOSTS_NO_PROXY

  (1..2).each do |i|
      config.vm.define "puppetagent0#{i}" do |puppetagent_config|
      puppetagent_config.vm.box = BOX_URI
      puppetagent_config.vm.network :private_network, ip: "10.1.172.1#{i}"    
      puppetagent_config.vm.hostname = "puppetagent0#{i}.osuk-puppet-lab.org"
      puppetagent_config.vm.provider "virtualbox" do |v, override|
        v.name = "puppetagent0#{i}"
        v.gui = false
        v.customize ["modifyvm", :id, "--memory", AGENT_BOX_MEM]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
        v.customize ["modifyvm", :id, "--cpus", "1"]      
        override.proxy.enabled = false
      end
      #puppetagent_config.vm.provision "shell",
      #inline: "rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm; yum install -y puppet 3.7.4-1"
    end
  end
        
  config.vm.define :puppetmaster do |puppetmaster_config|
    puppetmaster_config.vm.box = BOX_URI
    puppetmaster_config.vm.network :private_network, ip: "10.1.172.10"
    puppetmaster_config.vm.hostname = "puppetmaster.osuk-puppet-lab.org"
    puppetmaster_config.vm.provider "virtualbox" do |v, override|
      v.name = "puppetmaster"
      v.gui = true
      v.customize ["modifyvm", :id, "--memory", SERVER_BOX_MEM]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      override.proxy.enabled = false
    end
    #puppetmaster_config.vm.provision "shell",
    #inline: "rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm; yum install -y puppet-server 3.7.4-1"     
  end
  config.vm.provision :hosts do |provisioner|
        provisioner.add_host '10.1.172.10', ['puppetmaster.osuk-puppet-lab.org', 'puppet', 'puppetmaster']
        provisioner.add_host '10.1.172.11', ['puppetagent01.osuk-puppet-lab.org', 'puppetagent01']
        provisioner.add_host '10.1.172.12', ['puppetagent02.osuk-puppet-lab.org', 'puppetagent02']
  end
end