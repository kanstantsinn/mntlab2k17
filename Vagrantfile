# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
    web.vm.hostname = "web" 
    web.vm.network "private_network", ip: "172.16.1.10"
    web.ssh.insert_key = false
    web.vm.provider 'virtualbox' do |vb|
      vb.memory= "1024"
      vb.name = "web"
    end
  web.vm.provision 'shell', inline: <<-EOF
    sudo yum -y install epel-release wget net-tools unzip vim avahi avahi-tools
    sudo systemctl enable avahi-daemon
    sudo systemctl start avahi-daemon
    yum install -y nginx
    sudo cp -f /vagrant/lb.conf /etc/nginx/conf.d/
    systemctl enable nginx
    systemctl start nginx
    mkdir /home/vagrant/serf/
    cd /home/vagrant/serf/
    wget https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip
    unzip serf_0.8.1_linux_amd64.zip
    sudo cp -u /home/vagrant/serf/serf /bin/
    
  EOF
  end

(1..2).each do |i|
  config.vm.define "node#{i}" do |node|
    node.vm.box = "sbeliakou/centos-7.4-x86_64-minimal"
    node.vm.hostname = "node#{i}"
    node.vm.network "private_network", ip: "172.16.1.#{i+20}"
    node.ssh.insert_key = false
    node.vm.provider 'virtualbox' do |vb|
      vb.memory= "2048"
      vb.name = "node#{i}"
    end

  node.vm.provision 'shell', inline: <<-EOF
    sudo yum -y install epel-release wget net-tools unzip vim avahi avahi-tools java-devel
    sudo systemctl enable avahi-daemon
    sudo systemctl start avahi-daemon
    mkdir tomcat
    cd /home/vagrant/tomcat/
    wget http://ftp.byfly.by/pub/apache.org/tomcat/tomcat-8/v8.5.23/bin/apache-tomcat-8.5.23.tar.gz
    tar -xzvf  /home/vagrant/tomcat/apache-tomcat-8.5.23.tar.gz
    bash /home/vagrant/tomcat/apache-tomcat-8.5.23/bin/startup.sh
    mkdir /home/vagrant/serf/
    cd /home/vagrant/serf/
    wget https://releases.hashicorp.com/serf/0.8.1/serf_0.8.1_linux_amd64.zip
    unzip serf_0.8.1_linux_amd64.zip
    sudo cp -u /home/vagrant/serf/serf /bin/
  EOF
  end 
end

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

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
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
