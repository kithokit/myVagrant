# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  config.vm.define "ubu14" do |web|
# Every Vagrant virtual environment requires a box to build off of.
    web.vm.box = "trusty64"
    # The url from where the 'config.vm.box' box will be fetched if it
    # doesn't already exist on the user's system.
    web.vm.box_url = "http://files.vagrantup.com/precise64.box"
    web.vm.hostname = "vagrant-trusty"

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # config.vm.network :forwarded_port, guest: 80, host: 8080

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    # config.vm.network :private_network, ip: "192.168.33.10"

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    #config.vm.network :hostonly, "10.10.13.23", :netmask => "255.255.0.0"
    #web.vm.network "public_network", ip: "10.10.10.220"
    web.vm.network :public_network, :bridge => 'eth0'
    web.vm.network :private_network, :ip => '192.168.1.101'
    #config.vm.network :adapter, "1"
    #web.vm.network "10.10.10.220"

    web.vm.synced_folder "/Users/kithokit/Dropbox/", "/root/Dropbox"
    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    web.vm.provider :virtualbox do |vb|
    # # Use VBoxManage to customize the VM. For example to change memory:
       vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "4"]
    end
  end

    config.vm.define "cen64" do |cen|
    cen.vm.box = "centos_puppet"
    cen.vm.network :public_network, :bridge => 'eth0'
    cen.vm.network :private_network, :ip => '192.168.1.102'
    cen.vm.hostname = "centos64-puppet-test"
    cen.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "4"]
    end
    cen.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v0.1.0/centos64-x86_64-20131030.box"
    cen.vm.synced_folder "/home/kithokit/Dropbox/", "/root/Dropbox"
    cen.vm.synced_folder PUPPET_SOURCE, PUPPET_DEST
    cen.vm.synced_folder "templates" , '/tmp/vagrant-puppet/templates'
    cen.vm.provision "shell" do |s|
      s.path = "scripts/installpuppet.sh"
    end
    cen.vm.provision :puppet do |p|
      p.manifest_file  = "default.pp"
      p.options = ["--templatedir","/tmp/vagrant-puppet/templates"]
    end
  end

  config.vm.define "ubu12" do |ubu|
    ubu.vm.box = "ubu12_puppet"
    ubu.vm.network :public_network, :bridge => 'eth0'
    ubu.vm.network :private_network, :ip => '192.168.1.103'
    ubu.vm.hostname = "ubu12-puppet-test"
    ubu.vm.box_url = "http://files.vagrantup.com/precise64.box"
    ubu.vm.synced_folder "/home/kithokit/Dropbox/", "/root/Dropbox"
    ubu.vm.synced_folder PUPPET_SOURCE, PUPPET_DEST
    ubu.vm.synced_folder "templates" , '/tmp/vagrant-puppet/templates'
    ubu.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "4"]
    end
    ubu.vm.provision "shell" do |s|
      s.path = "scripts/installpuppet.sh"
    end
    ubu.vm.provision :puppet do |p|
      p.manifest_file  = "default.pp"
      p.options = ["--templatedir","/tmp/vagrant-puppet/templates"]
    end
  end

  config.vm.define "win082" do |win|
    win.vm.guest = :windows
    win.windows.halt_timeout = 25
    win.winrm.username = "vagrant"
    win.winrm.password = "vagrant"
    win.vm.network :forwarded_port, guest: 5985, host: 5985

    win.vm.box = "windows08r2_onboard"
    win.vm.box_url = "./win08r2.box"
    # Port forward WinRM and RDP
    win.vm.network :forwarded_port, guest: 3389, host: 3389
    win.vm.network :forwarded_port, guest: 5985, host: 5985
    win.vm.network :public_network, :bridge => 'eth0'
    win.vm.network :private_network, :ip => '192.168.1.105'
    win.vm.hostname = "win12-puppet-test"
    win.vm.synced_folder PUPPET_SOURCE, "/puppet"
    win.vm.synced_folder "templates" , "/tmp/vagrant-puppet/templates"
    win.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "4"]
    end
    #win.vm.provision :puppet do |p|
      #p.manifest_file  = "default.pp"
      #p.options = ["--templatedir","/tmp/vagrant-puppet/templates"]
    #end
    #win.vm.provision "shell" do |s|
      #s.path = "./installer/appstack-agent-installer-v3.9.ps1"
      #s.args = "#{API_KEY} #{SECRET_KEY} #{USER} #{CE_URL}"
    #end
  end
end
