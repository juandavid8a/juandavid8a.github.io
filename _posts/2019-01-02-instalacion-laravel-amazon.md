---
layout: post
title:  "Instalación Laravel SSH Servidor UBUNTU"
description: Comandos de consola paso a paso para instalar laravel en servidor UBUNTU
comments: true
category: Tutoriales
tags: Tutoriales Laravel
youtube: https://bit.ly/2wSo5iD
---

A continuación describo linea a linea lo que hay que hacer en la consola SSH para tener en menos de 10 minutos un proyecto Laravel corriendo en un servidor UBUNTU (AWS Amazon Web Services).

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso: 

```
sudo apt-get update
sudo apt-get install apache2 -y
echo $USER 
sudo nano /etc/apache2/envvars
export APACHE_RUN_USER=ubuntu
export APACHE_RUN_GROUP=ubuntu
sudo chown ubuntu:ubuntu /var/www/html -R
sudo a2enmod rewrite
sudo nano /etc/apache2/sites-available/000-default.conf

<Directory "/var/www/html/">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>

sudo systemctl restart apache2
sudo apt-add-repository ppa:ondrej/php
sudo apt-get update
sudo apt install php7.2 libapache2-mod-php7.2 php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-zip -y
sudo apt-get install git
cd /tmp
wget https://getcomposer.org/composer.phar
sudo mv composer.phar /bin/composer
sudo chmod 755 /bin/composer
composer global require "laravel/installer"
sudo nano ~/.profile

export PATH=$PATH:/home/ubuntu/.config/composer/vendor/bin

sudo reboot
cd /var/www/html/
laravel new aplication-sample

//arreglar error de swap en aws
free -m
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1

//error en unzip en composer.json
sudo apt install zip unzip php7.2-zip -y //segun la version de PHP

```
