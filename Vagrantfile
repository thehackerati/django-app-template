# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/trusty64"

    config.vm.network "private_network", ip: "192.168.59.103"
    config.vm.synced_folder ".", "/src"

    config.vm.provider "virtualbox" do |vb|
      vb.customize ['modifyvm', :id, "--natdnshostresolver1", "on"]
    end

    # Enable ssh forward agent
    config.ssh.forward_agent = true

    # Run apt-get update
    config.vm.provision :shell, inline: "apt-get update"

    # Install Ruby dev, which gem needs
    config.vm.provision :shell, inline: "sudo apt-get install python-software-properties"
    config.vm.provision :shell, inline: "sudo apt-add-repository ppa:brightbox/ruby-ng"
    config.vm.provision :shell, inline: "sudo apt-get update"
    config.vm.provision :shell, inline: "sudo apt-get -y install ruby2.2 ruby2.2-dev ruby-switch"
    config.vm.provision :shell, inline: "sudo ruby-switch --set ruby2.2"

    # Install travis CLI
    config.vm.provision :shell, inline: "gem install travis --no-rdoc --no-ri"
    config.vm.provision :shell, inline: "gem install travis-lint --no-rdoc --no-ri"

    # Install PIP
    config.vm.provision :shell, inline: "curl -O https://bootstrap.pypa.io/get-pip.py"
    config.vm.provision :shell, inline: "python get-pip.py"

    # Install AWS Elastic Beanstalk CLI
    config.vm.provision :shell, inline: "pip install awsebcli"

    # Install Docker, Docker Compose, and stand up the docker environment
    config.vm.provision :docker
    config.vm.provision :docker_compose, yml: "/src/docker-compose.yml", rebuild: true, project_name: "django-app-template", run: "always"
end
