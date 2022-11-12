# Server Creation

Here you can learn how to create a web server. <br>
This teaching is demonstrated by Laravel. <br>
I wrote teaching for the first time, and the explanation is not clear. Please tell me. Email: jianhuiwilliam80@gmail.com<br>

Tools used in this teaching <br>
Server provider: Microsoft Azure uses student packages here, which are free <br>
Tools:
Ngnix,
Php,
Composer

---

Now let's start our teaching <br>
Let's register our account at Microsoft Azure's <a href = "https://azure.microsoft.com/en-us/"> official website </a>

After successful registration, you need to open the student account mode. If your school has subscribe the Microsoft Azure<br>

After successful opening, we will display this screen.
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/1.png?raw=true)

Next, we click Virtual Machines. Click create in the upper left corner. We will enter the create interface.

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/2.png?raw=true)

We can fill out the form as follows.

1. Resource Group → Select "New" and use the name vm-machine-tutorial (whatever name you want).
2. Name → vm-machine-tutorial.
3. Area → Any Azure area close to you. (This service may not be available in some areas, so it can be replaced elsewhere).
4. Image → Ubuntu 20.04 (You can choose what you want, but the following teaching may be different. The choice here is Ubuntu 20.04)
5. Select the inbound port → check all of them, and then you can access the server through SSH.
   ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/4.png?raw=true)

---

Then we press View+Create in the lower left corner, and then select Create in the lower left corner.
<br> You will need to download SSH's Key when you create it. Let's download it. <br>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/7.png?raw=true)

---

When the following picture appears, it proves that our virtual machine is created <br>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/6.png?raw=true)

---

When we have created the virtual machine, open the command line. Enter the following command

```
SSH-I ~ path/VM-machine-tutorial _ key.pemazureuser@4.196.248.194 (followed by your IP)
```

Path Example = C:/Users/user/Desktop/vm-server.pem

Follow the location of your pem file <br/>
The first time you call, this will appear. Write down yes <br/>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/9.png?raw=true)<br />

Success will bring you to this interface <br/>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20032134.png?raw=true)<br />
Ok, start building the server now

The teaching is conducted with Laravel.

---

Let's learn a few Linux instructions before starting the build.

Check folders

```
ls -l (Viewing the files in the current folder does not include hidden files, and the files started by. git, .env, etc.)
```

```
ls -a (view the files in the current folder, including hidden files, and the starting files.)
```

Copy file

```
cp <file name> <The target name of the copy>
```

delete a file

```
rm <file name>
```

Clear the current instruction interface.

```
clear
```

edit file

```
nano <file name>
```

View files

```
cat <file name>
```

---

First, let's enter the administrator status.

```
sudo su
```

---

Next, we update the server file.

```
sudo apt update
```

---

Next, let's install Ngnix<br/>
Ngnix is a web server with asynchronous framework. It contains Apache, lighttpd and so on.

```
sudo apt install nginx
```

You will be asked whether to install the input Y.

You can operate Nginx through these four instructions. <br>
Ngnix common instructions

```
sudo systemctl stop Nginx (stop nginx)
sudo systemctl start Nginx (start nginx)
sudo systemctl status Nginx (check nginx status)
sudo systemctl reload Nginx (restart nginx)
```

You can check whether nginx is installed by the following command

```
nginx -v
```

---

Next, we install git.
Programmer must

```
sudo apt install git
```

You can check whether git is installed by the following command

```
git --version
```

---

Next, we install php and composer packages.

```
sudo apt -y install php7.4
```

You may see the red error after typing this, but you can ignore it.

```
sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath php-cli unzip
```

The above is to install php extension.

```
sudo apt install php-fpm
```

The above is the server configuration extension (uncertain)

---

Install Composer<br >
Follow the following instructions

```
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```

```
HASH=`curl -sS https://composer.github.io/installer.sig`
```

```
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

Complete installation

The following is for composer to be used globally, including administrators.

```
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

You can check whether composer is installed by the following command

```
composer --version
```

---

Install MySQL

```
sudo apt install mysql-server
```

---

So far, we have finished 80%

Let's reload ngnix and update the server files <br>
Use the following instructions

```
sudo systemctl reload nginx
```

```
sudo apt update
```

---

Next, we use git to copy a project to the server <br>
The location can be anywhere, but you must remember the location of the file <br>
I use my own Laravel project here.

```
git clone https://github.com/JianHui1208/Laravel-Project-Modules.git
```

```
cd Laravel-Project-Modules
```

---

Here we will configure Laravel.
Copy an env file

```
cp .env.example .env
```

```
nano .env
```

In env, we will set

Database name

Database user name

Database user password

URL (put the IP of the server in APP_URL)

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/11.png?raw=true)<br />

Next, we enter the following instructions

```
composer install
```

```
php artisan key:generate
```

---

Because we haven't created the database now, we can't perform php artisan migrate <br >
Next, we create the database.

Enter in the same place.

```
mysql
```

You will enter mysql's server, and we will use sql to create users and databases.

```
CREATE USER 'user1'@'%' IDENTIFIED BY 'aaa1234zzz.'; (create user)
```

```
GRANT ALL PRIVILEGES ON *.* TO 'user1'@'%'; (give user permission)
```

User1 is db \_ username just set in. env < br/>
Aaa1234zzz. is the DB_PASSWORD just set in. env

You can set any name and password you want.

Next, let's input

```
quit
```

Exit mysql server

We use the following instructions to enter mysql server as user1

```
mysql -u user1 -p
```

We will be asked to enter the password that was just set when the user was created.

When you enter the password, you will enter mysql server as user1.

We need to use a correct DB_USER to create the database, but not root.

Now we create the database.

```
CREATE DATABASE tutorial_database;
```

The most important thing about mysql is <b>; </b><br>
Be careful

Once created, we can enter

```
quit
```

---

Then we can

```
php artisan migrate --seed
```

---

Next, go to the most troublesome place <br>
We use our IP to look at it on the browser, and Apache's defualt interface will be displayed. If not, you can try to reload nginx.

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20192539.png?raw=true)<br />

---

Next, let's go to the front of the folder.

```
cd /
```

Enter the folder of nginx.

```
cd etc/nginx
```

We can use

```
ls -l
```

To check the files in the folder. Our main files here are sites-available and sites-enabled.

---

In sites-available, there is a file called default. This document is a web server configuration document. He is an example for you to copy a new file to change your desired website configuration. <br>
Sites-enabled is a symbolic link to the files in the available folders of the site.

---

Next, we can enter the sites-available folder and copy a default file.

```
Cp default tutorial (any name is fine, here "tutorial" is used)
```

---

Next, let's enter the file you copied.

```
nano tutorial
```

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20194822.png?raw=true)<br />

Next, let's follow the sequence step by step <br>
It is useless for us to use the mouse here. We need to use the arrow keys to control it.

We came to the following position

```
root /var/www/html;
```

Modify it to the path of Laravel file, and remember to add a public after it.

```
root /home/azureuser/Laravel-Project-Modules/public;
```

---

Next three lines down, come to

```
index index.html index.htm index.nginx-debian.html ;
```

At; Add index.php in front, and become

```
index index.html index.htm index.nginx-debian.html index.php ;
```

---

Next two lines, come to

```
server_name _;
```

Let's write in our IP address. Twice, in different ways.

```
server_name 4.196.248.194 http://4.196.248.194/;
```

---

Next, we come here.

```
try_files $uri $uri/ =404;
```

Delete =404 and add /index.php? $query_string becomes

```
try_files $uri $uri/ /index.php? $query_string;
```

---

Follow the picture below to delete the corresponding

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20200603.png?raw=true)<br />

---

Before saving, we can check the following pictures one more time.

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20200641.png?raw=true)<br />

---

Next, we will save it.

```
Press Ctrl+X, press y, and then press enter
```

---

Next, we need to link the tutorial file to the sites-enabled place.

We exit the sites-available folder and enter the sites-enabled folder.

```
cd ..
```

```
cd sites-enabled
```

Because default is linked to sites-enabled from the beginning, we need to unlink it first.

```
unlink default
```

You can check the folder and it will be empty.

```
ls -l
```

Next, connect the files (this step is similar to connecting the files on both sides, and if one side changes, both sides will change) (note the path)

```
ln -s /etc/nginx/sites-available/tutorial /etc/nginx/sites-enabled/
```

---

After successful connection, we reload nginx's service.

```
sudo systemctl reload nginx
```

If nothing appears, it proves success.

Now we use the IP address to go to the browser and open the Error interface that will be Laravel. This is a success.

---

Next, let's return to the command line.
We quit the folder of Nginx.
We came to Laravel's folder.

```
cd /
```

```
cd home/azureuser/Laravel-Project-Modules/
```

---

We enter the following three lines of instructions (give them permission)

```
sudo chown -R root:www-data storage bootstrap/cache
```

```
chmod -R 775 storage
```

```
chmod -R 775 bootstrap/cache
```

Then we use the Laravel command to clear the cache.

```
php artisan config:cache
```

---

So our web server has been created.
