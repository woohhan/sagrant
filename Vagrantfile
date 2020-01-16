#!/usr/bin/ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

$hosts = {"node1" => "192.168.30.101"}
#          "node2" => "192.168.30.102",
#          "node3" => "192.168.30.103"}
$extra_disks = {"disk1" => 10 * 1024,
                "disk2" => 10 * 1024,
                "disk3" => 10 * 1024}
$bridge = "enp24s0"

Vagrant.configure("2") do |config|
    config.vm.provider :virtualbox do |vb|
        vb.memory = "2048"
        vb.cpus = 2
    end

	$hosts.each do |name,ip|
        config.vm.define "#{name}" do |host|
            host.vm.hostname = "#{name}"
            host.vm.box = "ubuntu/bionic64"
            host.vm.network "private_network", ip: "#{ip}"
        end
            $extra_disks.each do |disk, size|
                config.vm.provider :virtualbox do |vb|
                    dev = ".vagrant/#{name}-#{disk}.vdi"
                    port = 2 + $extra_disks.find_index {|k,_| k=="#{disk}"}

                    unless File.exist?(dev)
                        vb.customize ['createhd', '--filename',
                        "#{dev}", '--size', "#{size}"]
                    end
                    vb.customize ['storageattach', :id,
                        '--storagectl', 'SCSI' , '--port', "#{port}",
                        '--device', 0, '--type', 'hdd',
                        '--medium', "#{dev}"]
                end
            end

    end

    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
            echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
        SHELL
      end
end

