## Multiple web pages configuration 
 Using single server (host name or IP) configured home web page and multiple alias web pages and also wecan configure multiple web pages on different port as per availability.

 ### Prerequsite 

- Linux server

### STEP 1:
To create a website we have to configure some settings

* Instlling HTTP package
Command to install HTTP pacakage is
```bash
sudo yum install -y httpd
```
* Verify that the http is insalled
```bash
sudo service httpd status
```
* Verify that the firewall is running or not
```bash
sudo systemctl status firewalld
```
* If firewall is not installed install firewall 
```bash
sudo yum install firewalld   
```
* Enable firewall
```bash
sudo systemctl enable firewalld
sudo systemctl start firewalld
```
* Now inorder to launch a webpage we require port 80 to be open so in order to open port 80 run below command.
```bash
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

### STEP 2:

Now we have to configre the rules and settings of HTTP so note down following contents to your notepad.
1. Server public IP addres
2. Hostname

* Now open  below path to configure
```bash
cd /etc/httpd/conf
```
* We have to edit some contents from below path
```bash
vim httpd.conf
```
* Now jump to last paragraph of the page
* Copy that paragraph and past it below.
* Now we have change some values.
```bash
<VirtualHost <IP ADDRESS OF UR SERVER>:80>
    ServerAdmin root@<Server host name>       
    DocumentRoot /var/www/html
    ServerName <Host name>
    ErrorLog logs/dummy-host.example.com-error_log
    CustomLog logs/dummy-host.example.com-access_log common
</VirtualHost>
```
* In first line add your server IP adress 
* In secondline add your server hostname
* Since we are using HTTP based websever all the resource required for the webserver will be stored in /var/www/html file
* Server name should be same as hostname

EX:
```bash
<VirtualHost 192.168.119.200:80>
    ServerAdmin root@shriram       
    DocumentRoot /var/www/html
    ServerName shriram
    ErrorLog logs/dummy-host.example.com-error_log
    CustomLog logs/dummy-host.example.com-access_log common
</VirtualHost>
```
* Save the file 
```bash
Esc+shift:wq!
```
### STEP 3:

Now we have to add content for the website

* Open this path
```bash
cd /var/www/html
```
* Open this file
```bash
vim index.html
```
* Add the content that should be displayed in website

```html
<style>
    .back{
        color: rgb(0, 162, 255);
        font-size: 18px;
    }

    .back:active{
        opacity: 0.3px;
    }

    .back:hover{
        color: red;
    }

    .Nike{
        color: black;
        font-size: 30px;
        font-weight: bolder;
    }

    .price{
        color:rgb(57, 118, 0);
        font-size: 20px;
        font-weight: bold;
    }

    .free{
        font-size: 20px;
        color: black;
    }

    .but1{
        background-color: rgb(255, 216, 20);
        font-weight: bold;
        border: none;
        height: 30px;
        width: 120px;
        border-radius: 20px;
        cursor: pointer;
        transition: backround-color 1s,color 1s;
    }

    .but1:active{
        opacity: 0.5px;
    }

    .but1:hover {
        background-color:rgb(221, 181, 5);
        color: black;
    }

    .but2{
        background-color: rgb(255,164,28);
        font-weight: bold;
        cursor: pointer;
        border: none;
        border-radius: 20px;
        height: 30px;
        width: 120px;
        transition: background-color 1s,color 1s;
    }

    .but2:hover{
        box-shadow: 5px 5px 10px rgba(0,0,0,0.5);
    }

</style>

<a class="back"   href="https://www.amazon.in/" target="_blank">
Back to Amazon
</a>

<p class="Nike">
    Nike Running Black Shoes
</p>

<p class="price">
    $39-in stock.

</p>

<p class="free">
    Free delivery tomorrow.
</p>

<button class="but1">
    Add to Cart
</button>

<button class="but2">
    Buy now
</button>
```
* Save the file
* Run below command to restart the HTTP service
```bash
service hhtpd restart
```
### STEP 4:

Open the web broserve and copy your IP address and paste.

![Screenshot 2023-08-07 135323](https://github.com/180172/Web_Pages_Configuration/assets/110009356/db10ef90-a705-4c26-9ef1-7b376dea9788)

```
You can acess the wesite with content which you have added on it.

### Suppose if we want to create subpage under same web-page use **alias**

```bash
<VirtualHost <IP ADDRESS OF UR SERVER>:80>
    ServerAdmin root@<Server host name>       
    DocumentRoot /var/www/html
    alias <alias name> /var/www/html/<alias name>
    ServerName <Host name>
    ErrorLog logs/dummy-host.example.com-error_log
    CustomLog logs/dummy-host.example.com-access_log common
</VirtualHost>
```
EX :
```bash
<VirtualHost 192.168.119.200:80>
    ServerAdmin root@shriram       
    DocumentRoot /var/www/html
    ServerName shriram
    alias busybox /var/www/html/busybox
    ErrorLog logs/dummy-host.example.com-error_log
    CustomLog logs/dummy-host.example.com-access_log common
</VirtualHost>
```

Now open this path /var/www/html/busybox and create new index.html file and add sub-page contents

After adding contents restart the service.

Open the browser paste URL like belo

```bash
http:/<server ip>:80/<alias name>
```

After all this if you want to create another new wepage follow the same process but insted of port 80 add any other port

EX: 
```
<VirtualHost 192.168.119.200:8080>
    ....................
    --------------------
    --------------------
    --------------------
</VirtualHost>
```
