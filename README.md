# Linux Server Configuration Project

> Robert Vo

## About

This is the final project for Udacity's Full Stack Web Developer Nanodegree. A baseline installation of Ubuntu Server on a virtual machine (Lightsail) is configured and prepared to host web applications. This includes installing updates, securing the server from attacks, and installing / configuring web and database servers.


# Server Info

- **Public IP:** 18.215.183.227
- **Port:** 2200
- Server Access `ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@18.215.183.227`
- App URL http://18.215.183.227/

## Getting Started

This project uses [Amazon Lightsail](https://amazonlightsail.com/) to create a Linux server instance.

1. Creating a baseline server.

    - Start a new Ubuntu Linux server instance on Amazon Lightsail. 
        
        * Register!
        * Create an instance image OS Only Ubuntu
        * Once the instance has started up, follow the instructions provided to SSH into your server.

2. Secure your server.

    - Update all currently installed packages.
    
            sudo apt-get update
            sudo apt-get upgrade
            
        auto upgrades run
            sudo dpkg-reconfigure --priority=low unattended-upgrades
    - Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
            
            sudo vim /etc/ssh/sshd_config
        change port form 22 to 2200
        
        
    - Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
        
            sudo ufw allow 2200/tcp
            sudo ufw allow 80/tcp
            sudo ufw allow 123/tcp
            sudo ufw enable

        Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. Review this video for details! When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.
        

3. Give grader access.

    In order for your project to be reviewed, the grader needs to be able to log in to your server.

    - Create a new user account named grader.
    
            sudo adduser grader
    - Create a new file under the suoders directory: 
    `$ sudo nano /etc/sudoers.d/grader`. 
    
    Fill that newly created file with the following line of text: "grader ALL=(ALL:ALL) ALL", then save it.
    
    - Configure the key-based authentication for *grader* user

    Generate an encryption key **on your local machine** with: `$ ssh-keygen -f ~/.ssh/udacity_key.rsa`.

    Log into the remote VM as *root* user through ssh and create the following file: `$ touch    /home/grader/.ssh/authorized_keys`.

    Copy the content of the *udacity_key.pub* file from your local machine to the */home/grader/.ssh/authorized_keys* file you  
    just created on the remote VM. Then change some permissions:
    
    1. `$ sudo chmod 700 /home/grader/.ssh`.
    2. `$ sudo chmod 644 /home/grader/.ssh/authorized_keys`.
    3. Finally change the owner from *root* to *grader*: `$ sudo chown -R grader:grader /home/grader/.ssh`.
    4. Now you are able to log into the remote VM through ssh with the following command: `$ ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@18.215.183.227`.

    - Enforce key-based authentication
    1. `$ sudo nano /etc/ssh/sshd_config`. Find the *PasswordAuthentication* line and edit it to *no*.
    2. `$ sudo service ssh restart`.
    
    - Disable ssh login for *root* user
    1. `$ sudo nano /etc/ssh/sshd_config`. Find the *PermitRootLogin* line and edit it to *no*.
    2. `$ sudo service ssh restart`.


4. Prepare to deploy your project.

    - Configure the local timezone to UTC.
    
            sudo dpkg-reconfigure tzdata
            
        * select none of the above, then UTC
    
    - Install Git
     1. `$ sudo apt-get install git`.
     2. Configure your username: `$ git config --global user.name <username>`.
     3. Configure your email: `$ git config --global user.email <email>`.
            
    - Install and configure Apache to serve a Python mod_wsgi application.
    
        ngnix is used instead of Apache
        
        configuration steps can be found here 
        
        https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04
        

    - Install and configure PostgreSQL:
    
            sudo apt-get install postgresql

5. Do not allow remote connections
    
    Create a new database user named catalog that has limited permissions to your catalog application database.
    
        https://help.ubuntu.com/community/PostgreSQL

6. Deploy the Item Catalog project.

    http://18.215.183.227/
    
## Resources


https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

https://help.ubuntu.com/community/PostgreSQL
