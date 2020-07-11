# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "centos/7",
        :net => [
                   {adapter: 2, auto_config: false, virtualbox__intnet: "router-net"},
                   {adapter: 3, auto_config: false, virtualbox__intnet: "router-net"}
               ]
  },

  :centralRouter => {
        :box_name => "centos/7",
        :net => [                 
                   {ip: '192.168.0.1', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: '192.168.0.33', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
                   {ip: '192.168.0.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "wi-fi-net"}, 
                   {adapter: 5, auto_config: false, virtualbox__intnet: "router-net"},
                   {adapter: 6, auto_config: false, virtualbox__intnet: "router-net"}
                ]
  },  

  :centralServer => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"}
                ]
  },

  :office1Router => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.254.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
               {ip: '192.168.2.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "dev-office1-net"},
               {ip: '192.168.2.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "testservers-office1-net"},
               {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
               {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "hardware-office1-net"},
               {ip: '192.168.3.1', adapter: 7, netmask: "255.255.255.192", virtualbox__intnet: "test-net"}
            ]
  },

  :office1Server => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.2.2', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "dev-office1-net"}
            ]
  },

  :testServer1 => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.3.2', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
               {adapter: 3, auto_config: false, virtualbox__intnet: "test-lan"}
            ]
  },
 
  :testClient1 => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.3.3', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
               {adapter: 3, auto_config: false, virtualbox__intnet: "test-lan"}
            ]
  },

  :testServer2 => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.3.4', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
               {adapter: 3, auto_config: false, virtualbox__intnet: "test-lan"}
            ]
  },
 
  :testClient2 => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.3.5', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
               {adapter: 3, auto_config: false, virtualbox__intnet: "test-lan"}
            ]
  },

  :office2Router => {
    :box_name => "centos/7",
    :net => [
              {ip: '192.168.253.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
              {ip: '192.168.1.1', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "dev-office2-net"},
              {ip: '192.168.1.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "testservers-office2-net"},
              {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "hardware-office2-net"}
            ]
  }, 
  
  :office2Server => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "dev-office2-net"}
            ]
  }

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
      
    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        config.vm.provider "virtualbox" do |v|
          v.memory = 256
        end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
      end

  end

  config.vm.provision "ansible" do |ansible|
  	ansible.playbook = "provision.yml"
	#ansible.verbose = "vv"
  end
  
end
