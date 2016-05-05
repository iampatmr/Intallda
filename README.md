# Intallda

##########################################################################

Install Directadmin

##########################################################################

yum install -y wget;wget --no-check-certificate https://raw.githubusercontent.com/janovas/Directadmin-Installer/master/install.sh -O install.sh;chmod 755 install.sh;./install.sh 2>&1|tee directadmin_install.log

##########################################################################

ติดตั้งเสร็จ enable firewall

##########################################################################

cd /usr/src
rm -fv csf.tgz
wget https://download.configserver.com/csf.tgz
tar -xzf csf.tgz
cd csf
sh install.sh

##########################################################################

mrtg ไว้ ดู traffic

##########################################################################

wget -qO - --no-check-certificate https://raw.githubusercontent.com/janovas/mrtg-auto-install/master/mrtg_da.sh | sh


##########################################################################

upgrade to CustomBuild 2.0

##########################################################################

cd /usr/local/directadmin/custombuild/
./build version
---------------------------------------------------------------------------
cd /usr/local/directadmin
mv custombuild custombuild_1.4
wget -O custombuild.tar.gz http://files.directadmin.com/services/custombuild/2.0/custombuild.tar.gz
tar xvzf custombuild.tar.gz
cd custombuild
./build

--------------------------------------------------------------------------
Config file options.conf
--------------------------------------------------------------------------
vi  options.conf
----------------------
php1_release=5.6 
php1_mode=suphp
php2_release=5.5
php2_mode=mod_php

--------------------------------------------------------------------------
บันทึก :wq
--------------------------------------------------------------------------

./build php
./build update_data
./build options
./build all

Also please add the following lines in your .htaccess of your domain which need to be loaded with CLI/mod_php

[root@tig01 custombuild]#vi /home/username/public_html/.htaccess   
===================
<FilesMatch "\.php$">
AddHandler application/x-httpd-php .php
</FilesMatch>
===================

cd /usr/local/directadmin/custombuild
./build options
./build update
./build set php5_ver 5.6
./build php n

##########################################################################

Install Softaculous

##########################################################################

cd /usr/local/directadmin/custombuild
./build set ioncube yes
./build ioncube

--------------------------------------------------------------------------

--------------------------------------------------------------------------
ROOT Directory
--------------------------------------------------------------------------

wget -N http://files.softaculous.com/install.sh
chmod 755 install.sh
./install.sh

##########################################################################

Install Composer + SSL FREE

##########################################################################

--------------------------------------------------------------------------
Install Composer
Skip this step if you already have Composer installed.
--------------------------------------------------------------------------

curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

--------------------------------------------------------------------------ไิิ
 Plugin
--------------------------------------------------------------------------
cd /usr/local/directadmin/plugins
git clone https://github.com/Petertjuh360/da-letsencrypt.git da-letsencrypt
cd ./da-letsencrypt/
chown diradmin:diradmin -hR ../da-letsencrypt/
sh ./scripts/install.sh
composer install
chown diradmin:diradmin -hR ../da-letsencrypt/
Change active=no and installed=no to active=yes and installed=yes in plugin.conf.
--------------------------------------------------------------------------
Update
--------------------------------------------------------------------------
cd /usr/local/directadmin/plugins/da-letsencrypt
git pull
composer update

##########################################################################

WEB SERVER USE NGINX

##########################################################################

cd /usr/local/directadmin/custombuild
./build set webserver nginx
./build set php1_mode php-fpm
./build update
./build all d
./build rewrite_confs

##########################################################################

enable_ssl_sni=1
##########################################################################
