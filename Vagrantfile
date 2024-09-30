# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/focal64"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.provision "shell", inline: <<-SCRIPT
      sudo apt update
      sudo apt upgrade -y
      sudo apt-get install -y nginx
      sudo apt install python3-flask
      cp /vagrant/nginx.conf /etc/nginx
      sudo systemctl reload nginx
    SCRIPT
  end
  config.vm.define "db" do |db|
    db.vm.box = "debian/bookworm64"
    db.vm.network "forwarded_port", guest: 80, host: 8081
  end
end
config.vm.define "database" do |database|
  database.vm.box = "ubuntu/focal64"
  database.vm.network "forwarded_port", guest: 5432, host: 5432
  database.vm.provision "shell", inline: <<-SCRIPT
    sudo apt update
    sudo apt upgrade -y
    sudo apt-get install -y postgresql postgresql-contrib
    sudo systemctl start postgresql
    sudo -u postgres psql -c "CREATE USER vagrant WITH PASSWORD 'vagrant';"
    sudo -u postgres psql -c "CREATE DATABASE vagrantdb OWNER vagrant;"
  SCRIPT
end