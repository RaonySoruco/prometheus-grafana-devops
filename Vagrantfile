# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "prom-server" do |v|
    config.vm.provider "virtualbox" do |c|
      c.memory = 2048
    end
    v.vm.box = "boxcutter/centos72"
    v.vm.hostname = "prom-server.example.com"
    v.vm.network :private_network, ip: "192.168.100.10"
    v.vm.synced_folder  ".", "/vagrant"
    v.hostmanager.aliases = %w(prom-server)
    v.vbguest.auto_update = false
    v.vm.provision "shell", inline: <<-SHELL
      sudo yum update -y
      sudo yum install -y yum-utils
      sudo yum-config-manager --add-repo https://docs.docker.com/engine/installation/linux/repo_files/centos/docker.repo
      sudo yum -y install docker-engine
      sudo usermod -aG docker vagrant
      sudo bash -c "curl -L "https://github.com/docker/compose/releases/download/1.11.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose"
      sudo chmod +x /usr/local/bin/docker-compose
      sudo systemctl enable docker
      sudo systemctl start docker
    SHELL
  end

  config.vm.define "node" do |v|
    config.vm.provider "virtualbox" do |c|
      c.memory = 512
    end
    v.vm.box = "boxcutter/centos72"
    v.vm.hostname = "node.example.com"
    v.vm.network :private_network, ip: "192.168.100.11"
    v.hostmanager.aliases = %w(agent1)
    v.vbguest.auto_update = false
    v.vm.provision "shell", inline: <<-SHELL
      sudo yum update -y
      sudo yum install -y yum-utils
      sudo yum-config-manager --add-repo https://docs.docker.com/engine/installation/linux/repo_files/centos/docker.repo
      sudo yum -y install docker-engine
      sudo usermod -aG docker vagrant
      sudo bash -c "curl -L "https://github.com/docker/compose/releases/download/1.11.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose"
      sudo chmod +x /usr/local/bin/docker-compose
      sudo systemctl enable docker
      sudo systemctl start docker
    SHELL
  end

end
