$master_script = <<SCRIPT
#!/bin/bash

apt-get -q -y --force-yes install git
git clone https://github.com/jshiv/all-spark-cluster.git
cd all-spark-cluster
bash get-docker.sh
bash start-master.sh
SCRIPT

$worker_script = <<SCRIPT
#!/bin/bash

apt-get -q -y --force-yes install git
git clone https://github.com/jshiv/all-spark-cluster.git
cd all-spark-cluster
bash get-docker.sh
bash start-worker.sh spark://10.211.55.100:7077
SCRIPT

$client_script = <<SCRIPT
#!/bin/bash

apt-get -q -y --force-yes install git
git clone https://github.com/jshiv/all-spark-cluster.git
cd all-spark-cluster
bash get-docker.sh
bash start-client.sh
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
      v.name = "vm-cluster-master"
      v.customize ["modifyvm", :id, "--memory", "1024"]
    end
    master.vm.network :private_network, ip: "10.211.55.100"
    master.vm.network :public_network
    master.vm.hostname = "vm-cluster-master"
    master.vm.provision :shell, :inline => $hosts_script
    master.vm.provision :hostmanager
    master.vm.provision :shell, :inline => $master_script
  end

  config.vm.define :slave1 do |slave1|
    slave1.vm.box = "ubuntu/trusty64"
    slave1.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-worker1"
      v.customize ["modifyvm", :id, "--memory", "1024"]
    end
    slave1.vm.network :private_network, ip: "10.211.55.101"
    slave1.vm.hostname = "vm-cluster-worker1"
    slave1.vm.provision :shell, :inline => $hosts_script
    slave1.vm.provision :hostmanager
    slave1.vm.provision :shell, :inline => $worker_script
  end

  config.vm.define :client do |client|
    client.vm.box = "ubuntu/trusty64"
    client.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-client"
      v.customize ["modifyvm", :id, "--memory", "1024"]
    end
    client.vm.network :private_network, ip: "10.211.55.104"
    client.vm.hostname = "vm-cluster-client"
    client.vm.provision :shell, :inline => $hosts_script
    client.vm.provision :hostmanager
    client.vm.provision :shell, :inline => $client_script
  end

end
