# -*- mode: ruby -*-
# vi: set ft=ruby :

veos = 'arista/veos'

Vagrant.configure(2) do |config|
  config.vm.boot_timeout = 500

  config.vm.define 'spine01' do |node|
    node.vm.box = veos
    node.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

    node.vm.network 'forwarded_port', guest: 22, host: 2222, id: 'ssh'

    node.vm.network 'private_network', virtualbox__intnet: 's1l1', auto_config: false
    node.vm.network 'private_network', virtualbox__intnet: 's1l2', auto_config: false

    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-vms"]
      v.customize ["modifyvm", :id, "--nicpromisc3", "allow-vms"]
    end

    #Configure Hostname, Username + SSH Key (Vagrant insecure), and IP Addressing
    node.vm.provision "shell", inline: <<-SHELL
      sleep 5
      FastCli -p 15 -c "configure
      hostname spine01
      ip domain-name lab.angrynet.ninja
      username ansible privilege 15 secret SuperSecretPassword01 role network-admin
      username ansible sshkey ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key
      interface Ethernet1
        no switchport
        ip address 10.0.0.0/31
      interface Ethernet2
        no switchport
        ip address 10.0.0.2/31
      ip routing
      ip route 0.0.0.0/0 10.0.2.2
      router bgp 65000
        neighbor 10.0.0.1 remote-as 65001
        neighbor 10.0.0.3 remote-as 65002
      ip name-server 1.1.1.1
      ntp server 220.233.156.30
      wr mem"
      SHELL

  end

  config.vm.define 'leaf01' do |node|
    node.vm.box = veos
    node.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

    node.vm.network 'forwarded_port', guest: 22, host: 2223, id: 'ssh'

    node.vm.network 'private_network', virtualbox__intnet: 's1l1', auto_config: false

    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-vms"]
    end

    #Configure Hostname, Username + SSH Key (Vagrant insecure), and IP Addressing
    node.vm.provision "shell", inline: <<-SHELL
      sleep 5
      FastCli -p 15 -c "configure
      hostname leaf01
      ip domain-name lab.angrynet.ninja
      username ansible privilege 15 secret SuperSecretPassword01 role network-admin
      username ansible sshkey ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key
      interface Ethernet1
        no switchport
        ip address 10.0.0.1/31
      ip routing
      ip route 0.0.0.0/0 10.0.2.2
      router bgp 65001
        neighbor 10.0.0.0 remote-as 65000
      ip name-server 1.1.1.1
      ntp server 220.233.156.30
      wr mem"
      SHELL

  end

  config.vm.define 'leaf02' do |node|
    node.vm.box = veos
    node.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

    node.vm.network 'forwarded_port', guest: 22, host: 2224, id: 'ssh'

    node.vm.network 'private_network', virtualbox__intnet: 's1l2', auto_config: false

    node.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-vms"]
    end

    #Configure Hostname, Username + SSH Key (Vagrant insecure), and IP Addressing
    node.vm.provision "shell", inline: <<-SHELL
      sleep 5
      FastCli -p 15 -c "configure
      hostname leaf02
      ip domain-name lab.angrynet.ninja
      username ansible privilege 15 secret SuperSecretPassword01 role network-admin
      username ansible sshkey ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key
      interface Ethernet1
        no switchport
        ip address 10.0.0.3/31
      ip routing
      ip route 0.0.0.0/0 10.0.2.2
      router bgp 65002
        neighbor 10.0.0.2 remote-as 65000
      ip name-server 1.1.1.1
      ntp server 220.233.156.30
      wr mem"
      SHELL

  end

end
