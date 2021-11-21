#Variables
hostnetworkkbridge = 'wlp2s0'
ansible_server = 'ansible'
defaultroute = "192.168.50.1"

hosts =[
  {
    :name => "ansible",
    :ip => "192.168.50.229"
  },
  {
    :name => "master",
    :ip => "192.168.50.230"
  },
  {
    :name => "node1",
    :ip => "192.168.50.231"
  },
  {
    :name => "node2",
    :ip => "192.168.50.232"
  }
]



Vagrant.configure("2") do |config|
 
  config.vm.box = "ubuntu/impish64"
  config.ssh.insert_key = false

  config.vm.provision "shell", inline: <<-SHELL
    #sudo route delete default
    #sudo route add default gw "#{defaultroute}"
    #sudo sed -i '/precedence ::ffff:0:0.96  100/s/^#//g' /etc/gai.conf 

    sudo cp /vagrant/insecure_private_key  /home/vagrant/insecure_private_key
    sudo chown vagrant:vagrant /home/vagrant/insecure_private_key
    sudo chmod 600 /home/vagrant/insecure_private_key

    sudo apt update --yes
    sudo apt upgrade --yes
 SHELL


  hosts.each do |host|
    config.vm.provision "shell", inline: "grep #{host[:name]} /etc/hosts || sudo sed -i '$a#{host[:ip]} #{host[:name]}' /etc/hosts"
  end

  hosts.each do |host|
    config.vm.define host[:name] do |node|
      node.vm.hostname = host[:name]
      node.vm.network "public_network", bridge: "#{hostnetworkkbridge}", ip: host[:ip]
      #node1.vm.network "public_network", bridge: "#{hostnetworkkbridge}", ip: "192.168.50.231"
      #config.ssh.guest_port = 2201
      #config.ssh.guest_port = 2201
      #config.ssh.guest_port = 2201
      #node1.vm.network "forwarded_port", guest: 22, host: 2201

      node.vm.provider "virtualbox" do |v|
        v.name = host[:name]
        v.memory = 5000
        v.cpus = 2
      end
    end
  end

  config.vm.define "#{ansible_server}" do |ansible_server|
    ansible_server.vm.provision "ansible_local" do |ansible|
      ansible.verbose  = true
      #ansible.verbose  = "vvvv"
      ansible.limit  = "nodes"
      #ansible.playbook_command = "sudo ansible-playbook"
      #ansible.config_file = "ansible.cfg"
      #ansible.playbook = "playbook.yml"
      #ansible.playbook = "ubuntuPXEserver.yml"
      ansible.playbook = "kubernetes.yml" 
      ansible.inventory_path = "inventory.yml"  
    end
  end 
end
    