# 伺服器建立

在这里你可以学到怎么样创建一个伺服器。<br>
本教学是使用 Laravel 进行演示。<br>
第一次写教学，解释得不清楚请多多保函<br>

本教学使用的工具<br>
伺服器服务商：Microsoft Azure 这里使用的是学生配套，免费 👍<br>
工具：
Ngnix,
Php,
Composer

---

现在开始我们的教学<br>
我们先在 Azure 的<a href="https://azure.microsoft.com/en-us/">官方网站</a>进行账号注册<br>

注册成功后，需要去开启学生账户模式。<br>

开启成功后，我们会显示这个画面。
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/1.png?raw=true)

接下来我们点击 Virtual Machines(虚拟机)。点击左上角的 create。我们就会进到 create 的界面。

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/2.png?raw=true)

我们可以跟着以下所述填写表单

1. 资源组 → 选择“新建”，并使用名称 vm-machine-tutorial (你想要什么名字都行)。
2. 名称 → vm-machine-tutorial。
3. 区域 → 与你靠近的任何 Azure 区域。(有些地区可能没有该服务，可以更换至其他地方)。
4. 映像 → Ubuntu 20.04 (可以选择自己想要的，但是下面的教学可能会不同。这里的选择是 Ubuntu 20.04)
5. 选择入站端口 → 全部打勾到完，你才可以通过 SSH 进到伺服器。
   ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/4.png?raw=true)

---

接着我们就按左下角 查看 + 创建，然后在左下角再选择创建。
<br>创建时会需要下载 SSH 的 Key，我们把他下载下来。<br>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/7.png?raw=true)

---

当出现以下图片时就证明我们的虚拟机创建完成<br>
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/6.png?raw=true)

---

当我们创建好了虚拟机后，打开命令行。输入以下命令

```
ssh -i ~path/vm-machine-tutorial_key.pem azureuser@4.196.248.194 (后面是你的IP)
```

Path Example = C:/Users/user/Desktop/vm-server.pem

需要跟着你的 pem 文件的位置<br />
第一次打会出现这个 写下 yes <br />
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/9.png?raw=true)<br />

成功了就会进到这个界面<br />
![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20032134.png?raw=true)<br />
ok 现在就开始 build server

该教学是使用 Laravel 进行

---

在开始 build 之前 我们先学会几个 Linux 指令

检查文件夹

```
ls -l 查看当前文件夹里的文件 不包括隐藏文件，.开始的文件 (例如.git, .env等等)
```

```
ls -a 查看当前文件夹里的文件 包括隐藏文件，.开始的文件
```

复制文件

```
cp 文件名称 复制的目标名称
```

删除文件

```
rm 文件名称
```

清除当前指令界面

```
clear
```

修改文件

```
nano 文件名称
```

查看文件

```
cat 文件名称
```

---

首先我们先进入管理员身份

```
sudo su
```

---

接下来我们更新伺服器文件

```
sudo apt update
```

---

接下来我们安装 Ngnix<br />
Ngnix 是异步框架的网页服务器。里面包含了 Apache、lighttpd 等等。

```
sudo apt install nginx
```

会问你是否安装 输入 Y

你可以通过这个四个指令，对 Nginx 进行操作。<br>
Ngnix 常用指令

```
sudo systemctl stop nginx (停止 Nginx)
sudo systemctl start nginx (开始 Nginx)
sudo systemctl status nginx (检查 Nginx 状况)
sudo systemctl reload nginx (重新启动 Nginx)
```

可以透过以下指令，查看 nginx 是否安装完成

```
nginx -v
```

---

接下来我们安装 git
程序员必备

```
sudo apt install git
```

可以透过以下指令，查看 git 是否安装完成

```
git --version
```

---

接下来我们安装 php 和 composer 包

```
sudo apt -y install php7.4
```

输入完这个你可能看到红色错误，但是可以不必理会它

```
sudo apt-get install -y php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath php-cli unzip
```

以上是安装 php extension

```
sudo apt install php-fpm
```

以上是 server 配置 extension (不确定)

---

安装 Composer<br>
跟着以下的指令

```
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```

```
HASH=`curl -sS https://composer.github.io/installer.sig`
```

```
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

安装完成

以下是为了让 composer 能够全局使用，包括管理员

```
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

可以透过以下指令，查看 composer 是否安装完成

```
composer --version
```

---

安装 MySQL

```
sudo apt install mysql-server
```

---

到这里为止我们完成了 80%

让我们来重新 reload ngnix 和更新伺服器文件<br>
使用以下指令

```
sudo systemctl reload nginx
```

```
sudo apt update
```

---

接下来我们用 git 复制一个项目来到伺服器<br>
位置可以在任何地方 但是必须记得文件的位置<br>
我这里使用我自己的 Laravel 项目

```
git clone https://github.com/JianHui1208/Laravel-Project-Modules.git
```

```
cd Laravel-Project-Modules
```

---

这里我们就进行 Laravel 的配置
复制一个 env 文件

```
cp .env.example .env
```

```
nano .env
```

在 env 里面 我们会设置

数据库名称

数据库用户名称

数据库用户密码

网站 URL (把伺服器的 IP 放到 APP_URL)

![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/11.png?raw=true)<br />

接下来我们输入以下指令

```
composer install
```

```
php artisan key:generate
```

---

因为我们现在没有创建数据库 所以不可以进行 php artisan migrate <br>
接下来我们创建数据库

在同样的地方输入

```
mysql
```

就会进入到 mysql 的 server 我们会使用 sql 来进行创建用户和数据库

```
CREATE USER 'user1'@'%' IDENTIFIED BY 'aaa1234zzz.'; (创建用户)
```

```
GRANT ALL PRIVILEGES ON *.* TO 'user1'@'%'; (给予用户权限)
```

user1 是刚刚设定在.env 的 DB_USERNAME<br />
aaa1234zzz.是刚刚设定在.env 的 DB_PASSWORD

你可以设定任何你想要的名字和密码

接下来我们先输入

```
quit
```

退出 mysql server

我们使用以下指令使用 user1 的身份进入 mysql server

```
mysql -u user1 -p
```

会要求我们输入密码 刚刚在创建用户时设定的密码

当你输入密码完成后 就会以 user1 的身份进入 mysql server

我们需要使用一个正确的 DB_USER 来创建数据库 不可以使用 root

现在我们创建数据库

```
CREATE DATABASE tutorial_database;
```

mysql 最重要的就是<b> ；</b><br>
要小心

创建好了我们就可以输入

```
quit
```

---

接下来我们就可以

```
php artisan migrate --seed
```

---

接下来就到最麻烦的地方<br>
我们使用我们的 IP 在浏览器上看，会显示 Apache 的 defualt 界面。如果没有，可以试着 reload nginx。

## ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20192539.png?raw=true)<br />

---

接下来我们去到文件夹的最前端

```
cd /
```

进入 nginx 的文件夹里

```
cd etc/nginx
```

我们可以使用

```
ls -l
```

来查看文件夹里面的文件。在这里我们主要的文件是 sites-available 和 sites-enabled

---

在 sites-available 里面，有一个文件是叫 default。这个文件是网站伺服器配置文件。他是一个例子，让你来复制新的一个文件来更改你想要的网站配置。<br>
sites-enabled 则是指向站点可用文件夹中文件的符号链接。

---

接下来我们可以进入到 sites-available 的文件夹里复制一个 default 的文件

```
cp default tutorial(任何名字都可以，这里使用的是"tutorial")
```

---

接下来我们进入到你复制出来的文件里

```
nano tutorial
```

## ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20194822.png?raw=true)<br />

接下来我们跟着顺序一步一步来 <br>
在这里我们用滑鼠是没有用的 我们需要使用方向键来控制

我们来到以下的位置

```
root /var/www/html;
```

把他修改成 Laravel 文件的路径，记得需要在后面加一个 public

```
root /home/azureuser/Laravel-Project-Modules/public;
```

---

接下来往下三行，来到

```
index index.html index.htm index.nginx-debian.html;
```

在;的前面加上 index.php，变成

```
index index.html index.htm index.nginx-debian.html index.php;
```

---

接下来往下两行，来到

```
server_name _;
```

我们把我们的 IP 地址写进去。两次，不同的写法

```
server_name 4.196.248.194 http://4.196.248.194/;
```

---

接下来我们来到这里

```
try_files $uri $uri/ =404;
```

把=404 删除掉，加/index.php?$query_string 变成

```
try_files $uri $uri/ /index.php?$query_string;
```

---

跟着以下图片删除对应的#

## ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20200603.png?raw=true)<br />

---

在保存之前我们可以检查多一次跟着以下的图片

## ![alt text](https://github.com/JianHui1208/Tutorial_Notepad/blob/main/IMG/Screenshot%202022-11-02%20200641.png?raw=true)<br />

---

接下来我们进行保存

```
按下 Ctrl + X，按下 y，再按下 enter
```

---

接下来我们需要把 tutorial 的文件 link 到 sites-enabled 的地方

我们退出 sites-available 的文件夹，进入到 sites-enabled 的文件夹里

```
cd ..
```

```
cd sites-enabled
```

因为 default 在一开始就 link 着 sites-enabled 的地方，我们需要先进行 unlink

```
unlink default
```

可以检查文件夹，会显示是空的

```
ls -l
```

接下来连接文件(这一步是类似把两边的文件进行连接，其中一边进行更改，两边都会有变化)(注意路径)

```
ln -s /etc/nginx/sites-available/tutorial /etc/nginx/sites-enabled/
```

---

连接成功后，我们 reload nginx 的 service

```
sudo systemctl reload nginx
```

如果没有出现任何东西就证明成功了

现在我们使用 IP 地址去到浏览器打开会是 Laravel 的 Error 界面。这样就是成功了。

---

接下来我们回到命令行里。
我们退出 Nginx 的文件夹里。
我们来到 Laravel 的文件夹。

```
cd /
```

```
cd home/azureuser/Laravel-Project-Modules/
```

---

我们输入以下三行指令 (赋予权限给它们)

```
sudo chown -R root:www-data storage bootstrap/cache
```

```
chmod -R 775 storage
```

```
chmod -R 775 bootstrap/cache
```

然后我们使用 Laravel 指令清除 cache

```
php artisan config:cache
```

---

这样我们的网站就创建完成了。
