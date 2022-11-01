Vagrant.configure("2") do |config|
    config.vm.box = "joyce/new-box"
    config.vm.box_version = "1.0.0"
    config.vm.hostname = "database"
    config.vm.box_url = "file:///home/joyce/f3a07af0-d168-45d3-9060-ba7d070f4f30"
    config.vm.boot_timeout = 360
    config.ssh.username = "sf_user"
    config.ssh.password = "sf_user"
    config.vm.provider "virtualbox" do |vb|
        vb.name = "postgres_8.4"
        vb.customize ["modifyvm", :id, "--memory", 1024 * 4]
    end
    config.vm.network "forwarded_port", guest: 5432, host: 5432
    config.vm.provision "shell", inline: <<-SHELL
        wget https://www.postgresql.org/media/keys/ACCC4CF8.asc
        sudo apt-key add ACCC4CF8.asc
        sudo echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main 8.4" >> /etc/apt/sources.list.d/pgdg.list
        sudo apt-get update
        sudo DEBIAN_FRONTEND=noninteractive apt-get install -qq postgresql-8.4 postgresql-client-8.4 postgresql-contrib-8.4 < /dev/null > /dev/null
        sudo -u postgres psql postgres -c "ALTER USER postgres WITH ENCRYPTED PASSWORD 'postgres'"
        sudo -u postgres psql postgres -c "CREATE USER sf_user WITH ENCRYPTED PASSWORD 'sf_user'"
        sudo -u postgres psql postgres -c "CREATE DATABASE vagrant OWNER sf_user"
        sudo echo "listen_addresses = '*'" >> /etc/postgresql/8.4/main/postgresql.conf
        sudo echo "logging_collector = on" >> /etc/postgresql/8.4/main/postgresql.conf
        sudo service postgresql restart
    SHELL
end
