# -*- mode: ruby -*-
# vi: set ft=ruby :

$ip = "192.168.56.101"
$hostname = "single-hdfs"
$fqdn = "single-hdfs.example.com" # Look `ansible/files/hadoop/core-site.xml` line 20
$hadoop_version = "3.3.1"

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vbguest.installer_options = { allow_kernel_upgrade: true }

  config.vm.hostname = $hostname

  config.vm.network :forwarded_port, guest: 9870, host: 9870   # NameNode
  config.vm.network :forwarded_port, guest: 8088, host: 8088   # ResourceManager
  config.vm.network :forwarded_port, guest: 19888, host: 19888 # MapReduce JobHistory Server

  config.vm.network :private_network, ip: $ip

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = "2048"
  end
  
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "ansible/playbook.yaml"
    ansible.limit = "all"
    ansible.extra_vars = {
      username: "vagrant",
      ip: $ip,
      hostname: $hostname,
      fqdn: $fqdn,
      hadoop_version: $hadoop_version
    }
  end
end
