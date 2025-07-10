Vagrant.configure("2") do |config|
    
    config.vm.synced_folder "./shared", "/shared"

    config.vm.define "master" do |master|
        master.vm.box = 'bento/ubuntu-22.04'
        master.vm.network "private_network", ip: '192.168.56.110'
        master.vm.hostname = "master"
        master.vm.provider 'virtualbox' do |vb|
            vb.memory = 1024
            vb.cpus = 1
            vb.gui = false
        end
        master.vm.provision "shell", inline: <<-SHELL
            apt-get update
            apt-get upgrade -y
            curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--node-name master" sh -
            sudo cat /var/lib/rancher/k3s/server/node-token > /shared/node-token
        SHELL
    end
    config.vm.define "worker1" do |worker|
        worker.vm.box = "bento/ubuntu-22.04"
        worker.vm.network "private_network", ip: "192.168.56.111"
        worker.vm.hostname = 'worker1'
        worker.vm.provider "virtualbox" do |vb|
            vb.memory = 1024
            vb.cpus = 1
            vb.gui = false
        end
        worker.vm.provision "shell", inline: <<-SHELL
                apt-get update
                apt-get upgrade -y
                echo "⏳ Waiting for master to be ready..."
                sleep 30
                TOKEN=$(cat /shared/node-token)
                curl -sfL https://get.k3s.io | K3S_URL=https://192.168.56.110:6443 K3S_TOKEN=$TOKEN sh -s - agent --node-name worker1
            SHELL
      end        
end
  