username=""
password=""
VAGRANT_BOX         = "generic/rhel9"
VAGRANT_BOX_VERSION = "4.3.12"
CPUS_MASTER_NODE    = 2
CPUS_WORKER_NODE    = 2
MEMORY_MASTER_NODE  = 2048
MEMORY_WORKER_NODE  = 2048
WORKER_NODES_COUNT  = 2

Vagrant.configure(2) do |config|
    mastercount=3
    (1..mastercount).each do |i|
  
      config.vm.define "master#{i}" do |masternode|
      masternode.vm.provision "file", source: "/home/amit/.ssh/id_rsa.pub", destination: "~/.ssh/me.pub"
      masternode.vm.provision "shell", inline: <<-SHELL
        cat /home/vagrant/.ssh/me.pub >> /home/vagrant/.ssh/authorized_keys
      SHELL
        masternode.vm.box               = VAGRANT_BOX
        masternode.vm.box_check_update  = false
        masternode.vm.box_version       = VAGRANT_BOX_VERSION
        masternode.vm.hostname          = "master#{i}.cloudnativeapps.in"
  
        masternode.vm.network "private_network", ip: "172.16.16.10#{i}"
        masternode.vm.provider :virtualbox do |v|
          v.name   = "master#{i}"
          v.memory = 2048
          v.cpus   = 2
        end
        masternode.vm.provider :libvirt do |v|
          v.nested  = true
          v.memory  = 2048
          v.cpus    = 2
        end
        
      end
    end

    lbcount = 2
    (1..lbcount).each do |i|
      config.vm.define "loadbalancer#{i}" do |lb|
      lb.vm.provision "file", source: "/home/amit/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/me.pub"
      lb.vm.provision "shell", inline: <<-SHELL
        cat /home/vagrant/.ssh/me.pub >> /home/vagrant/.ssh/authorized_keys
      SHELL
        lb.vm.box               = "generic/rhel9"
        lb.vm.box_check_update  = false
        lb.vm.box_version       = VAGRANT_BOX_VERSION
        lb.vm.hostname          = "loadbalancer#{i}.cloudnativeapps.in"
   
        lb.vm.network "private_network", ip: "172.16.16.5#{i}"
   
        lb.vm.provider :virtualbox do |v|
          v.name   = "loadbalancer#{i}"
          v.memory = 512
          v.cpus   = 1
        end
   
        lb.vm.provider :libvirt do |v|
          v.memory  = 512
          v.cpus    = 1
        end
      end
   
    end
  
    (1..WORKER_NODES_COUNT).each do |i|
        config.vm.define "worker#{i}" do |machine|
        machine.vm.provision "file", source: "/home/amit/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/me.pub"
        machine.vm.provision "shell", inline: <<-SHELL
          cat /home/vagrant/.ssh/me.pub >> /home/vagrant/.ssh/authorized_keys
        SHELL
        machine.vm.box               = VAGRANT_BOX
        machine.vm.box_check_update  = false
        machine.vm.box_version       = VAGRANT_BOX_VERSION
        machine.vm.hostname          = "worker#{i}.cloudnativeapps.in"
        machine.vm.network "private_network", ip: "172.16.16.20#{i}"
        machine.vm.provider :virtualbox do |v|
          v.name    = "worker#{i}"
          v.memory  = MEMORY_WORKER_NODE
          v.cpus    = CPUS_WORKER_NODE
        end
    
        machine.vm.provider :libvirt do |v|
          v.memory  = MEMORY_WORKER_NODE
          v.nested  = true
          v.cpus    = CPUS_WORKER_NODE
        end
        end 
     end

     
end
