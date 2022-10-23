# -*- mode: ruby -*-
# vi: set ft=ruby :
# v3.0
# Author: Joe Axberg
# Comment:  Create a LAMP Stack and Installs the PHP Crude CRUD App
#           Creates two servers: one for the DB, one for the App server
#           Option to build on an ARM-based M1/M2 Mac

Vagrant.configure("2") do |config|
    
  #define a  do block for the web server 
  config.vm.define "web" do |web|
  
  # Use an appropriate box here!!!!! Uncomment the line applicable to you!!!!
  # For Intel Windows/Mac using Virtualbox (and only Virtualbox) use
  # In other words for an Intel PC, uncomment the line below and comment (or remove)
  # the lines for an ARM based Mac...
  web.vm.box = "ubuntu/focal64"

  # For ARM aka "M1/M2" aka "Apple Silicon" Macs using VMware Fusion for Apple Silicon
  # You must use a special box/template created by Joe
  #config.vm.box = "axnetlabs/axnetlabs_focal_arm64"   # line needed for ARM Macs
  #config.vm.provider 'vmware_desktop' do |vmware|     # line needed for ARM Macs
  #    vmware.gui = true                               # line needed for ARM Macs
  #end                                                  # line needed for ARM Macs

  # Do any networking if needed
  # You could port forward to the webserver
  # Or connect it to a host only network
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  #####for host only make sure you know what you are doing - ask for help!!!
  web.vm.network "private_network", ip: "192.168.56.11"
    
  # the following stuff installs the Apache web server
  #this block will copy a php test file to test php if needed
  #comment it out if you don't
  web.vm.provision "file", source: "phptest.php", destination: "phptest.php"

  #run a bunch of shell command to create the server and grab the PHP Crude CRUD App.
  web.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get install -y apache2
     apt-get install -y php libapache2-mod-php php-mysql
     systemctl restart apache2.service
     #cp the phptest.php only if needed or you want it
     #comment the next line out if you don't
     cp phptest.php /var/www/html
     echo "loading php web app"
     git clone https://github.com/axbjos/phpcrudecrudapp.git
     cd phpcrudecrudapp
     mv * /var/www/html
     SHELL

end   #end of the web server do block
  
#another block that defines the db server.
#we'll call it simply "db"
config.vm.define "db" do |db|

  # Use an appropriate box here!!!!! Uncomment the line applicable to you!!!!
  # For Intel Windows/Mac using Virtualbox (and only Virtualbox) use
  # In other words for an Intel PC, uncomment the line below and comment (or remove)
  # the lines for an ARM based Mac...
  db.vm.box = "ubuntu/focal64"

  # For ARM aka "M1/M2" aka "Apple Silicon" Macs using VMware Fusion for Apple Silicon
  # You must use a special box/template created by Joe
  # db.vm.box = "axnetlabs/axnetlabs_focal_arm64"   # line needed for ARM Macs
  # db.vm.provider 'vmware_desktop' do |vmware|     # line needed for ARM Macs
  #    vmware.gui = true                               # line needed for ARM Macs
  #end                                                  # line needed for ARM Macs

  # Do any networking if needed
  # You could port forward to the webserver
  # Or connect it to a host only network
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  #####for host only make sure you know what you are doing - ask for help!!!
  db.vm.network "private_network", ip: "192.168.56.10"
     
  #stage the mysql config file, the line below will copy the file from your laptop
  #to the VM and put it in the /home/vagrant director (aka Vagrant's home directory)
  db.vm.provision "file", source: "50-server.cnf", destination: "50-server.cnf"
  db.vm.provision "file", source: "addusers.sql", destination: "addusers.sql"

  #mysql provisioning
  db.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y mariadb-server
    #clone the db schema and data
    git clone https://github.com/datacharmer/test_db.git
    cd  test_db
    #load the db schema and data
    mysql -t < employees.sql
    cd ..
    #only do this if you want the db accessible via the outside world
    cp 50-server.cnf /etc/mysql/mariadb.conf.d/
    systemctl restart mariadb
    cd /home/vagrant
    #show the user a status message so
    echo "securing mysql and adding joeaxberg user"
    #run this script - it does what mysql_secure_installation does
    #and adds a user to the database that PHP will use
    mysql -t < addusers.sql
    echo "done"
  SHELL #end of shell command block
  
end  #endof the db block
