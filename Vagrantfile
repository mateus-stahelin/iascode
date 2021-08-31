Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 1
  end

  config.vm.define "wordpress" do |wordpress|
    wordpress.vm.network "public_network", ip: "10.193.40.98"
    wordpress.vm.provision "shell",
    inline: "cat /vagrant/configs/id_bionic_ho.pub >> .ssh/authorized_keys && \
             apt update -y"
    wordpress.vm.provider "virtualbox" do |vb|
      vb.name = "server_wordpress"
    end
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.network "public_network", ip: "10.193.40.99"
    ansible.vm.provision "shell",
    inline: "cp /vagrant/id_bionic_ho /home/vagrant/.ssh/ && \
             chmod 600 /home/vagrant/.ssh/id_bionic_ho && \
             chown vagrant:vagrant /home/vagrant/.ssh/id_bionic_ho"
    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "server_ansible"
    end
    ansible.vm.provision "shell",
    inline: "apt update && \
             apt install -y software-properties-common && \
             apt-add-repository --yes --update ppa:ansible/ansible && \
             apt install -y ansible"
    ansible.vm.provision "shell",
    inline: "ansible-playbook -i /vagrant/hosts \
             /vagrant/provisioning.yml"
    end
end
