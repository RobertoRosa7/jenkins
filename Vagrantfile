Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_download_insecure = true

  config.vm.define "organizese" do |org|
    org.disksize.size = '30GB'
    org.vm.network "forwarded_port", guest: 4200, host: 4200
    org.vm.network "forwarded_port", guest: 8080, host: 8080 # jenkins
    org.vm.network "private_network", ip: "192.168.33.11"
    
    org.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.name = "organizese"
      vb.cpus = 2
    end
    
    org.vm.provision "shell", inline: "sudo apt-get update && \
    sudo apt-get install swapspace python python3-pip python-dev libmysqlclient-dev python3-venv \
    build-essential libssl-dev libffi-dev unzip ansible sshpass curl -y"

    org.vm.provision "shell", inline: "curl -fsSL https://get.docker.com -o get-docker.sh && \
    sudo bash get-docker.sh"

    # $script_mysql = <<-SCRIPT
    #   apt-get update && \
    #   apt-get install -y openjdk-8-jdk  mysql-server-5.7 && \
    #   mysql -e "create user 'devops'@'%' identified by 'mestre';"  && \
    #   mysql -e "create user 'devops_dev'@'%' identified by 'mestre';"  && \
    #   mysql -e "create database todo;" && \
    #   mysql -e "create database todo_dev;" && \
    #   mysql -e "create database test_todo_dev;" && \
    #   mysql -e "grant all privileges on *.* to devops@'%' identified by 'mestre';" && \
    #   mysql -e "grant all privileges on *.* to devops_dev@'%' identified by 'mestre';" 
    # SCRIPT

    # org.vm.provision "shell", inline: $script_mysql
    # org.vm.provision "shell", inline: "cat /configs/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
    # org.vm.provision "shell", inline: "service mysql restart"
    # org.vm.synced_folder "./configs", "/configs"

    # org.vm.provision "shell", inline: "chmod +x /vagrant/scripts/*"
    # org.vm.provision "shell", inline: "sudo /vagrant/scripts/docker.sh"
  end

  config.vm.define "web-organizese-prod" do |prod|
    prod.vm.network "forwarded_port", guest: 80, host: 80
    prod.vm.network "private_network", ip: "192.168.33.10"
    
    config.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
      vb.name = "web-organizese-prod"
    end

    prod.vm.provision "shell", inline: "apt-get update && \
    apt-get upgrade -y \
    apt-get install -y nginx"
  end

  config.vm.define "api-organizese" do |api|
    api.vm.network "forwarded_port", guest: 5000, host: 5000
    api.vm.network "private_network", ip: "192.168.33.12"
    
    config.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
      vb.name = "api-organizese"
    end

    api.vm.provision "shell", inline: "sudo apt-get update && \
    sudo apt-get install swapspace python python3-pip python-dev libmysqlclient-dev python3-venv \
    build-essential libssl-dev libffi-dev unzip ansible sshpass -y"
  end

  config.vm.define "api-organizese-dev" do|api|
    api.vm.box = "ubuntu/focal64"
    api.vm.network "forwarded_port", guest: 5001, host: 5001
    api.vm.network "private_network", ip: "192.168.33.13"

    api.vm.provider "virtualbox" do |vb|
      vb.memory = 512
      vb.cpus = 1
      vb.name = "api-organizese-dev"
    end

    api.vm.provision "shell", inline: "sudo apt update; sudo apt upgrade -y"
    api.vm.provision "shell", inline: "sudo apt install apt-transport-https ca-certificates curl software-properties-common -y"
    api.vm.provision "shell", inline: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -"
    api.vm.provision "shell", inline: "sudo add-apt-repository 'deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable';"
    api.vm.provision "shell", inline: "sudo apt update; apt-cache policy docker-ce"
    api.vm.provision "shell", inline: "sudo apt install docker-ce"
  end
end
