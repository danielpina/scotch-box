# -*- mode: ruby -*-
# vi: set ft=ruby :

project_name = "myproject"

Vagrant.configure("2") do |config|
    
    # Fix for: "stdin: is not a tty"
    # https://github.com/mitchellh/vagrant/issues/1673#issuecomment-28288042
    config.ssh.shell = %{bash -c 'BASH_ENV=/etc/profile exec bash'}
    
    if Vagrant.has_plugin? 'vagrant-hostmanager'
        config.vm.network "private_network", ip: "192.168.33.10"
        config.vm.hostname = project_name + ".dev"
    else
        fail_with_message "vagrant-hostmanager missing, please install the plugin with this command:\nvagrant plugin install vagrant-hostsupdater"
    end

    config.vm.box = "scotch/box"
    # config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    
    # Optional NFS. Make sure to remove other synced_folder line too
    config.vm.synced_folder ".", "/var/www",  type: "nfs", nfs_udp: false, :nfs => { :mount_options => ["dmode=777","fmode=666"] }
    
    config.vm.provision "shell", inline: <<-SHELL
        # Modify vhost to load the main www dir, not the public subdir.
        sudo sed -i s,/var/www/public,/var/www,g /etc/apache2/sites-available/000-default.conf
        sudo sed -i s,/var/www/public,/var/www,g /etc/apache2/sites-available/scotchbox.local.conf
        sudo service apache2 restart

        # Install rocketeer
        wget http://rocketeer.autopergamene.eu/versions/rocketeer.phar
        chmod +x rocketeer.phar
        sudo mv rocketeer.phar /usr/local/bin/rocketeer

    SHELL

end
