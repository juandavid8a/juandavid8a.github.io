´´´´
sudo apt-get install apache2 -y
sudo apt-get install mysql-server -y
sudo mysql_secure_installation ()
sudo apt-get install php libapache2-mod-php php-mysql -y
sudo service apache2 restart
sudo chown root:root -R /var/www/
sudo chmod 777 -R /var/www/
sudo apt-get install phpmyadmin -y
sudo systemctl restart apache2
sudo mysql -u root -p 
CREATE USER 'ubuntu'@'localhost' IDENTIFIED BY 'tu_contrasena';
GRANT ALL PRIVILEGES ON * . * TO 'ubuntu'@'localhost';
FLUSH PRIVILEGES;
//solucionar error al entrar a las tablas
sudo cp /usr/share/phpmyadmin/libraries/sql.lib.php /usr/share/phpmyadmin/libraries/sql.lib.php.bak
sudo nano /usr/share/phpmyadmin/libraries/sql.lib.php
//buscar 
(count($analyzed_sql_results['select_expr'] == 1)
//remplazar por
((count($analyzed_sql_results['select_expr']) == 1)
´´´´
