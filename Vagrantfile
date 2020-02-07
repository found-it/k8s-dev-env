# -*- mode: ruby -*-
# vi: set ft=ruby :

IMAGE_NAME = "bento/ubuntu-16.04"
NODES = 2
MASTER_IP = "192.168.50.10"
WORKER_IP = "192.168.50."

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: MASTER_IP
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/master-playbook.yaml"
            ansible.extra_vars = {
                node_ip: MASTER_IP,
            }
        end
    end

    (1..NODES).each do |i|
        config.vm.define "k8s-worker#{i}" do |worker|
            worker.vm.box = IMAGE_NAME
            worker.vm.network "private_network", ip: "#{WORKER_IP}#{i + 10}"
            worker.vm.hostname = "k8s-worker#{i}"
            worker.vm.provision "ansible" do |ansible|
                ansible.playbook = "ansible/worker-playbook.yaml"
                ansible.extra_vars = {
                    node_ip: "#{WORKER_IP}#{i + 10}",
                }
            end
        end
    end
end

