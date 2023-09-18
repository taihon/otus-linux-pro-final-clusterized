# -*- mode: ruby -*-
# vim: set ft=ruby :
home = ENV['HOME']
ENV["LC_ALL"] = "en_US.UTF-8"

MACHINES = {
    :balancer1=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'10.0.10.2', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :balancer2=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'10.0.10.3', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :dbnode1=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.2', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :dbnode2=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.3', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :dbnode3=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.4', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :prxnode1=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.7', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :prxnode2=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.8', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :apnode1=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.5', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :apnode2=>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "1024",
        :net => [
            {ip:'192.168.11.6', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :backup =>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "256",
        :net => [
            {ip:'172.16.1.3', adapter:2,netmask:"255.255.255.0"}
        ]
    },
    :fw =>{
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "256",
        :net => [
            {ip:'10.0.10.10', adapter:2,netmask:"255.255.255.0"},
	    {ip:'172.16.1.10', adapter:3,netmask:"255.255.255.0"},
#            {ip:'172.16.1.5', adapter:5,netmask:"255.255.255.0"},
            {ip:'192.168.11.10', adapter:4,netmask:"255.255.255.0"}
        ],
	:ports => [
	    {guest: 443, host: 8082}
	]
    },
    :monitoring => {
        :box_name => "centos/7",
        :box_version => "2004.1",
        :memory => "3500",
        :net => [
            {ip:'172.16.1.2', adapter:2,netmask:"255.255.255.0"}
        ],
        :ports => [
            {guest: 9093, host: 9093},
            {guest: 9090, host: 9090}
        ]
    }
}

Vagrant.configure("2") do |config|

    config.vm.box_version = "2004.01"
    MACHINES.each_with_index do |(boxname, boxconfig), index|
        config.vm.synced_folder ".","/vagrant", disabled: true
        config.vm.define boxname do |box|
  
          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
          box.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", boxconfig[:memory]]
            vb.customize ["modifyvm", :id, "--cpus", "1"]
          end
          boxconfig[:net].each do |ipconf|
            box.vm.network "private_network", ipconf
          end
          if boxconfig.key?(:ports)
	    boxconfig[:ports].each do |portconf|
              box.vm.network "forwarded_port", portconf
	    end
	  end
          if boxconfig.key?(:public)
#            box.vm.network "public_network" boxconfig[:public]
          end
  
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh
            cp ~vagrant/.ssh/auth* ~root/.ssh
            echo "net.ipv4.conf.all.forwarding=1" >> /etc/sysctl.d/netforward.conf
            echo "net.ipv4.conf.all.rp_filter=2" >> /etc/sysctl.d/netforward.conf
            sysctl --system > /dev/null
          SHELL
          
#        if index == MACHINES.size - 1
#           box.vm.provision "ansible" do |ansible|
#            ansible.playbook = "ansible/playbook.yml"
#            ansible.limit = "all"
#            ansible.inventory_path = "ansible/hosts.yml"
#            ansible.host_key_checking = false
#          end
#        end
    	end
  	end
end
