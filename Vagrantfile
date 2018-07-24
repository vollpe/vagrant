# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "debian/stretch64"
    config.vm.box_check_update = false
    config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    config.vm.network "forwarded_port", guest: 3306, host: 3306
    config.vm.hostname = "localhost"
    config.vm.network "private_network", ip: "192.168.33.10"

#start of shell provisioning   
    config.vm.provision "shell", inline: <<-SHELL
    
#apps installation
    apt-get update && apt-get upgrade -y
    apt-get install -y apache2 php7.0 libapache2-mod-php7.0
    sudo service apache2 restart
    apt-get install -y mysql-server mysql-client php7.0-mysql

#project movement to apache shared folder
    if ! [ -L /var/www/html ]; then
    rm -rf /var/www/html
    ln -fs /vagrant/app /var/www/html
    fi

#mysql config for default user 'vagrant'
    `sudo mysql -e "\
    CREATE USER 'vagrant'@'localhost' IDENTIFIED BY 'vagrant'; \
    GRANT ALL PRIVILEGES ON * . * TO 'vagrant'@'localhost'; \"`
    sudo service mysql restart
    sudo service apache2 restart
    sudo mysql < /var/www/html/data/init.sql
    SHELL
end
