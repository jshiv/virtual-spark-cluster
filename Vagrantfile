$master_script = <<SCRIPT
#!/bin/bash

apt-get update
apt-get -q -y --force-yes install git
git clone https://github.com/Lab41/ipython-spark-docker.git
cd ipython-spark-docker
./0-prepare-host.sh
./2-run-spark-master.sh
SCRIPT

$slave_script = <<SCRIPT
#!/bin/bash

apt-get update
apt-get -q -y --force-yes install git
git clone https://github.com/Lab41/ipython-spark-docker.git
cd ipython-spark-docker
./0-prepare-host.sh
./3-run-spark-worker.sh spark://10.211.55.100:7077
SCRIPT

$hosts_script = <<SCRIPT
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

EOF
SCRIPT

Vagrant.configure("2") do |config|

  # Define base image
  config.vm.box = "ubuntu/trusty64"

  # Manage /etc/hosts on host and VMs
  config.hostmanager.enabled = false
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  config.hostmanager.ignore_private_ip = false

  config.vm.define :master do |master|
    master.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node1"
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end
    master.vm.network :private_network, ip: "10.211.55.100"
    master.vm.network :public_network
    master.vm.hostname = "vm-cluster-node1"
    master.vm.provision :shell, :inline => $hosts_script
    master.vm.provision :hostmanager
    master.vm.provision :shell, :inline => $master_script
  end

  config.vm.define :slave1 do |slave1|
    slave1.vm.box = "ubuntu/trusty64"
    slave1.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node2"
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end
    slave1.vm.network :private_network, ip: "10.211.55.101"
    slave1.vm.hostname = "vm-cluster-node2"
    slave1.vm.provision :shell, :inline => $hosts_script
    slave1.vm.provision :hostmanager
    slave1.vm.provision :shell, :inline => $slave_script
  end

  config.vm.define :slave2 do |slave2|
    slave2.vm.box = "ubuntu/trusty64"
    slave2.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node3"
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end
    slave2.vm.network :private_network, ip: "10.211.55.102"
    slave2.vm.hostname = "vm-cluster-node3"
    slave2.vm.provision :shell, :inline => $hosts_script
    slave2.vm.provision :hostmanager
    slave2.vm.provision :shell, :inline => $slave_script
  end

  config.vm.define :slave3 do |slave3|
    slave3.vm.box = "ubuntu/trusty64"
    slave3.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node4"
      v.customize ["modifyvm", :id, "--memory", "2048"]
    end
    slave3.vm.network :private_network, ip: "10.211.55.103"
    slave3.vm.hostname = "vm-cluster-node4"
    slave3.vm.provision :shell, :inline => $hosts_script
    slave3.vm.provision :hostmanager
    slave3.vm.provision :shell, :inline => $slave_script
  end

end
