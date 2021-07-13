## 1. Deploy your server on Vultr, Digital Ocean or choice of you. 
Deploy your server so that you will be ready to ssh. I will be using Debian 10 for this tutorial. It comes with lots of pre required and even sudo.

## 2. SSH into the server.
I use [cmder](https://cmder.net/) when on windows, else the shell. You are free to use Putty too.

## 3. First thing first, UPDATE the instance
The instance are created from iso images, so better update. ```$ apt-get update```

Now install sudo
``` apt-get install sudo ```

Create a new user account using the adduser command. Don’t forget to replace username with your desired user name:

```adduser username```
The command will prompt you to set and confirm the new user password. Make sure that the password for the new account is as strong as possible (combination of letters, numbers and special characters).
```
root@HIRA:~# adduser hiraschool
Adding user `hiraschool' ...
Adding new group `hiraschool' (1000) ...
Adding new user `hiraschool' (1000) with group `hiraschool' ...
Creating home directory `/home/hiraschool' ...
Copying files from `/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for hiraschool
Enter the new value, or press ENTER for the default
        Full Name []: Hira School
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] Y
```

Now Add the user to the sudo group
By default on Debian systems, members of the group sudo are granted with sudo access. To add a user to the sudo group use the usermod command:
```usermod -aG sudo username```

Exit and close the session and connect as the user you created.

Use the sudo command to run the whoami command:
```
sudo whoami
hiraschool@HIRA:~$ sudo whoami

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for hiraschool:
[sudo] password for hiraschool:
root
```


## 4. Install some components we will be needing on the way.
Debian already comes with lots of pre install component but we need some like zsh and git. Lets do it
```sudo apt-get install -y git zsh curl apt-transport-https```

## 5. Install zsh and make things pretty. :)
Want to know more about zsh? Check [zsh](https://ohmyz.sh/)
```$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"```
```$ vim ~/.zshrc``` and change the theme line to what you want, blinks is nice. Reload the shell: ```$ source ~/.zshrc```
If you want a nicer Vim experience, copy .vimrc in this repo to the root's home directory.

## 6. Install Nginx
First, let's check what version is the latest in the apt repository: $ apt-cache show nginx (it is most likely behind) Visit [http://nginx.org/en/linux_packages.html](Nginx repositories page) to see how to add the repository to our apt.

```
$ sudo apt-get update && sudo apt-get install -y nginx
$ sudo service nginx start
$ ip a
```
ip a  will give you the ip. Open the Ip on browser and you should see Nginx. If this is the first time you are installing Nginx Congratulations!

## 7. Create website directory inside www
cd into ```/var/www/``` and create your website. In my case i will type ```shamin.duckdns.org```
```mkdir shamin.duckdns.org``
``ls``
``cd shamin.duckdns.org```

## 8. Upload the HTML Files to the directory using WINSCP
upload the files to the directory using WINSCP. Login using root this time to upload.


## Step 4: Configure NGINX to serve your website
You’ll need to tell NGINX about your website and how to serve it.
cd into ```/etc/nginx/```. This is where the NGINX configuration files are located.
The two directories we are interested are ```sites-available``` and ```sites-enabled```.

```sites-enabled``` contains links to the configuration files that NGINX will actually read and run.

What we’re going to do is create a configuration file in ```sites-available```, and then create a symbolic link (a pointer) to that file in ```sites-enabled``` to actually tell NGINX to run it.

Create a file called ```jgefroh.com``` in the sites-available directory and add the following text to it:

```
server {
       listen 80;
       listen [::]:80;

       server_name tharaf.duckdns.org;

       root /var/www/tharaf.duckdns.org;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```

## Run a config test
```sudo service nginx configtest```
it must show OK..

## Reload or Restart server
``` sudo service nginx reload ```
