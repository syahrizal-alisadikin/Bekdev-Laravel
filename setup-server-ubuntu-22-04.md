Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara Setup server ubuntu-22-04,

<h3>Mengatur Firewall</h3>

https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu

    ```
    ufw app list
    ```
    
    ```
    ufw allow OpenSSH
    ```
    
    ```
    ufw enable
    ```
    
    ```
    ufw status
    ```

<h3>Install LEMP</h3>

1. Install Nginx
   
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
   
    ```
    sudo apt update
    ```
     
    ``` 
    sudo apt install nginx
    ```
     
    ``` 
    sudo ufw allow 'Nginx HTTP'
    ```
     
    ``` 
    sudo ufw status
    ```
     
    ``` 
    systemctl status nginx
    ```
     
    ``` 
    curl -4 icanhazip.com
    ```
     
    ``` 
    http://your_server_ip
    ```
     
    
     ![](https://i.imgur.com/Rd2PPvS.png)
   
2. Install MySQL
   
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-22-04
   
    ``` 
   sudo apt install mysql-server
    ```
  
  (Optional)
    ``` 
  sudo mysql_secure_installation
    ``` 
  
    Ganti ke metode password
   
    ```
    sudo mysql 
    ```
    
    ```
    SELECT user,authentication_string,plugin,host FROM mysql.user;
    ```
  
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
    ```

    Nanti masuknya pake: 
    ```
    mysql -u root -p
    ```

4. Install PHP

```
sudo apt install php8.2-fpm php8.2-mysql
```
 
6. Testing PHP

a. Masuk Ke Sites available
```
sudo nano /etc/nginx/sites-available/default
```
   
  b. Ubah ke :
  
```
index index.php index.html index.htm index.nginx-debian.html;
```

c. Tambahin lagi :

```
location ~ \.php$ {
   include snippets/fastcgi-php.conf;
   fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
}
location ~ /\.ht {
   deny all;
} 
```
   
d. Lalu di terminal
    
```
sudo nano /var/www/html/info.php
```

e. isi kode di dalam info.php
   
```
<?php
  phpinfo();
```
