# -*- mode: ruby -*-
# vi: set ft=ruby :
# vagrant files for k8s cluster(one master node, two worker nodes)
# edited by lee

VAGRANTFILE_API_VERSION = "2"

k8s_cluster = {
  "worker1" => { :ip => "192.168.90.20", :cpus => 1, :memory => 2048 },
  "worker2" => { :ip => "192.168.90.30", :cpus => 1, :memory => 2048 },
	"master"=> { :ip => "192.168.90.10", :cpus => 2, :memory => 4096 },
}
 
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  k8s_cluster.each do |hostname, info|

    config.vm.define hostname do |cfg|
      cfg.vm.provider "virtualbox" do |vb,override|
        config.vm.box = "generic/centos9s"
        override.vm.network "private_network", ip: "#{info[:ip]}"
        override.vm.host_name = hostname
        vb.name = hostname
				vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", info[:memory], "--cpus", info[:cpus]]
				if "#{hostname}" == "master" then
					override.vm.provision "shell", path: "ssh_conf.sh", privileged: true
					override.vm.provision "shell", path: "install_cluster.sh", privileged: true
					override.vm.provision "shell", path: "run_in_master.sh", privileged: true
					override.vm.provision "shell", path: "account.sh", privileged: false
					override.vm.provision "shell", path: "send_pub_key.sh", privileged: false
				else
					override.vm.provision "shell", path: "ssh_conf.sh", privileged: true
					override.vm.provision "shell", path: "install_cluster.sh", privileged: true
				end  
      end  
    end  
  end  
end 

