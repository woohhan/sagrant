#!/usr/bin/ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

$hosts = {"node1" => "192.168.30.101",
          "node2" => "192.168.30.102",
          "node3" => "192.168.30.103"}
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
    end

    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
            echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
            echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
        SHELL
      end
end

