#Variables
#hostnetworkkbridge = 'Ethernet: Realtek PCIe GBE Family Controller'


Vagrant.configure("2") do |config|
 
  config.vm.box = "ubuntu/xenial64"
  config.ssh.insert_key = false

  config.vm.provision "shell", inline: "sudo apt update"
  config.vm.provision "shell", inline: "sudo apt upgrade"
  config.vm.provision "shell", inline: "grep controlserver /etc/hosts || sudo sed -i '$a192.168.50.230 controlserver' /etc/hosts"
  config.vm.provision "shell", inline: "grep controlserver /etc/hosts || sudo sed -i '$a192.168.50.231 node1' /etc/hosts"
  config.vm.provision "shell", inline: "grep controlserver /etc/hosts || sudo sed -i '$a192.168.50.232 node2' /etc/hosts"
  


  config.vm.define "controlserver" do |control|
    control.vm.hostname = 'controlserver'
    control.vm.network "public_network", ip: "192.168.50.230"
    #, :type => "bridge", :dev => "bridge", :mode => "bridge"
    #config.ssh.guest_port = 2200
    #control.vm.network "forwarded_port", guest: 22, host: 2200

    control.vm.provider "virtualbox" do |v|
      v.name = "controlserver"
      v.memory = 5000
      v.cpus = 2
    end   
=begin
    control.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end 
=end
  end
  
  config.vm.define "node1" do |node1|
    node1.vm.hostname = 'node1'
    node1.vm.network "public_network", ip: "192.168.50.231"
    #node1.vm.network "public_network", bridge: "#{hostnetworkkbridge}", ip: "192.168.50.231"
    #config.ssh.guest_port = 2201
    #config.ssh.guest_port = 2201
    #config.ssh.guest_port = 2201
    #node1.vm.network "forwarded_port", guest: 22, host: 2201

    node1.vm.provider "virtualbox" do |v|
      v.name = "node1"
      v.memory = 5000
      v.cpus = 2
    end
  end

  config.vm.define "node2" do |node2|
    node2.vm.hostname = 'node2'
    node2.vm.network "public_network", ip: "192.168.50.232"
    #node2.vm.network "forwarded_port", guest: 22, host: 2202

    node2.vm.provider "virtualbox" do |v|
      v.name = "node2"
      v.memory = 5000
      v.cpus = 2
    end
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
