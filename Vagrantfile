# choose ubuntu version, focal / 20.04 or jammy / 22.04 ...

IMAGE_NAME = "ubuntu/jammy64"

# check versions here e.g.
# curl -s https://packages.cloud.google.com/apt/dists/kubernetes-xenial/main/binary-amd64/Packages | grep Version | awk '{print $2}'

K8S_VERSION = "1.25.3"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

# control node needs 2cpu and 2gb, so dont go under
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end

    config.vm.define "control" do |control|
        control.vm.box = IMAGE_NAME
        control.vm.network "private_network", ip: "10.10.1.10"
        control.vm.hostname = "control"
        config.vm.synced_folder "examples/resources/", "/opt/resources"

        control.vm.provision "ansible" do |ansible|
            ansible.playbook = "control-playbook.yml"
            ansible.extra_vars = {
                node_ip: "10.10.1.10",
                k8s_version: K8S_VERSION,
                ansible_python_interpreter: "/usr/bin/python3",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "worker-#{i}" do |worker|
            worker.vm.box = IMAGE_NAME
            worker.vm.network "private_network", ip: "10.10.1.#{i + 10}"
            worker.vm.hostname = "worker-#{i}"

# workers with 1cpu and 1gb work, but if you have plenty left, adjust it
	    worker.vm.provider "virtualbox" do |workerv|
  	    	workerv.memory = 1024
	    	workerv.cpus = 1
	    end

            worker.vm.provision "ansible" do |ansible|
                ansible.playbook = "worker-playbook.yml"
                ansible.extra_vars = {
                    control_node_ip: "10.10.1.10",
                    node_ip: "10.10.1.#{i + 10}",
                    k8s_version: K8S_VERSION,
                    ansible_python_interpreter: "/usr/bin/python3",
                }
            end
        end
    end
end

