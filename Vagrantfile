Vagrant.configure("2") do |config|
    ####    NUMBER OF WORKER NODES
    MasterNodeCPU = 4
    MasterNodeRAM = 2048

    WorkerNodeCount = 2
    WorkerNodeCPU = 2
    WorkerNodeRAM = 1024

    ####    KMASTER NODE
    ####
    config.vm.define "kmaster" do |kmaster|
        kmaster.vm.box = "centos/7"
        kmaster.vm.hostname = "kmaster.cluster.local"

        kmaster.vm.provider "virtualbox" do |vm|
            vm.name = "kmaster"
            vm.memory = MasterNodeRAM
            vm.cpus = MasterNodeCPU
        end

        kmaster.vm.network "private_network",
            ip: "10.0.0.1"

        kmaster.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/k8-deploy-master.yml"
        end
    end

    ####    KWORKER NODES
    ####
    (1..WorkerNodeCount).each do |i|
        config.vm.define "kworker-#{i}" do |kworker|
            kworker.vm.box = "centos/7"
            kworker.vm.hostname = "kworker-#{i}.cluster.local"

            kworker.vm.provider "virtualbox" do |vm|
                vm.name = "kworker-#{i}"
                vm.memory = WorkerNodeRAM
                vm.cpus = WorkerNodeCPU
            end

            kworker.vm.network "private_network",
                ip: "10.0.10.#{i}"

            kworker.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/k8-deploy-worker.yml"
            end
        end
    end

end