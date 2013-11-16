# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
#VAGRANTFILE_API_VERSION = "2"
#Must have newer than 4.3 virtualbox
Vagrant.require_plugin "vagrant-vbguest"
Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "saucy64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"


  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  # config.vm.box_url = "http://domain.com/path/to/above.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
   #config.vm.network :private_network, 
   #config.vm.network :dhcp
   #config.vm.network :hostonly, "10.11.12.13", :netmask = "255.255.255.0
   config.vm.network "private_network", ip: "192.168.56.15"
   config.vm.network "private_network", ip: "192.168.56.15"
   config.vm.network "forwarded_port", guest: 111, host: 111
   config.vm.network "forwarded_port", guest: 1110, host: 1110
   config.vm.network "forwarded_port", guest: 2049, host: 2049
   config.vm.network "forwarded_port", guest: 4045, host: 4045
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
   #config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # cfg.vm.share_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "./vagrantsync", "/vagrant"
  #config.vm.synced_folder "./root", "/","id: "vagrant-root" , nfs: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.
 # Configuration for Virtualbox provider
  config.vm.provider "virtualbox" do |vb|
    #vb.destroy_unused_network_interfaces = true
    # Virtualbox Name
    vb.customize ["modifyvm", :id, "--name", "Tomato-VM", "--ostype", "Ubuntu_64"]
    # Memory
    vb.customize ["modifyvm", :id, "--memory", "4092"]
	#CPU up to 4 cores and ioapic
	vb.customize ["modifyvm", :id, "--ioapic", "on"]
	vb.customize ["modifyvm", :id, "--cpus", "4"]
	vb.customize ["modifyvm", :id, "--pae", "on"]
    # Chipset (Supposedly better CPU performance)
    vb.customize [ "modifyvm", :id, "--chipset", "ich9" ]
    # NIC 1 (Better TCP over NAT performance, at least on Windows)
	vb.customize ["modifyvm", :id, "--nic1", "nat", "--nictype1", "virtio"] 
	vb.customize ["modifyvm", :id, "--natsettings1", "9000,1024,1024,1024,1024"]  
    # NIC 2 (Host Only Access)
	vb.customize ["modifyvm", :id, "--nic2", "hostonly", "--nictype2", "virtio"] 
    #vb.customize ["modifyvm", :id, "--hostonlyadapter2", :interface]
	#config.vm.network :hostonly, "10.10.10.10"
	#begin
	#  vb.customize ["dhcpserver", "modify", "--netname", "hostonly", "--enable", "--ip", "10.10.10.0", "--netmask", "255.255.255.254", "--lowerip", "10.10.10.1", "--upperip", "10.10.10.1"]
    #rescue
	#  vb.customize ["dhcpserver", "add", "--netname", "hostonly", "--enable", "--ip", "10.10.10.0", "--netmask", "255.255.255.254", "--lowerip", "10.10.10.1", "--upperip", "10.10.10.1"]
	#end  
	# Storage Controller
    #  NOTE: name "SATA" for some images,  "SATA Controller" for others
    # Add Second Drive
    # You need to have an environment variable set to the where you want the VM storage path or it will go in pwd
    #  Virtualbox VM path. i.e. type %VBOX_USER_HOME%\VirtualBox.xml | findstr defaultMachineFolder
    # i.e. set VBOX_VM_PATH=C:\NoAVScans\Vbox_VMs
    #if ENV["VBOX_VM_PATH"]
    #  disk2_path = ENV["VBOX_VM_PATH"] + "/" + $suggested_hostname + "/" + "box-disk2" + ".vmdk"
    #else
      disk2_path = Dir.pwd() + "/" + "Tomato-Data" + ".vmdk"
    #end

    vb.customize ["storagectl", :id, "--name", "SATAController", "--controller", "IntelAHCI", "--portcount", "2", "--hostiocache", "on"]
    vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "0", "--device", "0", "--nonrotational", "on"]
    vb.customize ["createhd", "--filename", disk2_path, "--size", 300*1024, "--format", "vmdk", "--variant", "Standard"]
    vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "1", "--device", "0", "--type", "hdd", "--medium", disk2_path]
  end

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file saucy64.pp in the manifests_path directory.
  #
  # An example Puppet manifest to provision the message of the day:
  #
  # # group { "puppet":
  # #   ensure => "present",
  # # }
  # #
  # # File { owner => 0, group => 0, mode => 0644 }
  # #
  # # file { '/etc/motd':
  # #   content => "Welcome to your Vagrant-built virtual machine!
  # #               Managed by Puppet.\n"
  # # }
  #
  # Guest addons fix
  

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = "../my-recipes/cookbooks"
  #   chef.roles_path = "../my-recipes/roles"
  #   chef.data_bags_path = "../my-recipes/data_bags"
  #   chef.add_recipe "mysql"
  #   chef.add_role "web"
  #

config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path = ""
	puppet.manifest_file  = "default.pp"
	puppet.options = "include nfs::server::ubuntu"
    puppet.options = ['--verbose']
end
  #   # You may also specify custom JSON attributes:
  #   chef.json = { :mysql_password => "foo" }
  # end
  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
end
