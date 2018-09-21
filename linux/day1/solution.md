# TASK1 Install and configure apache/httpd

    -sudo apt-get install apache2                                  // to install the apache                                    
    -whereis apache2                                               // to see which files got configured while installing apache
    
    
 ![](media/apache_install.png)

# TASK2 Install and configure nginx - configure it to run as reverse proxy to apache

## STEP1
    -sudo apt-get install nginx                                                          // to install the nginx 
    -whereis nginx                                                                       // to see the location of the files to execute nginx. For ex-configuration file,libraries etc

![](media/nginx_install.png)

## STEP 2     
                                       
Change the port for the apache server to 8085 in ports.conf file

    -sudo vi /etc/apache2/ports.conf

 - add port number using this line in file for ex-  Listen 8085

After modifying the file restart apache using command 
 
    -sudo systemctl restart apache2

![](media/apache_port.png)

## Step3

Now using Nginx as reverse proxy for apache

     -sudo vi /etc/nginx/sites-avaible/default
     
- by giving the server_ip and port and at which apache2 is running by adding this line
   
   location /{

     proxy_pass http://localhost:8085         // proxy_pass  http://server_ip:port of the backend server
         }

![](media/reverse_proxy_setup.png)

## Step4

Edit index.html using

    -sudo vi /var/www/html/index.html
    
  write Hi, I’m Devops Ninja in body of html page 

![](media/index_html.png)

After doing all of the above process restart nginx and apache

    -sudo systemctl restart nginx
    -sudo systemctl restart apache2

And open your browser and type localhost  and you can see here nginx as a reverse proxy for apache 

![](media/reverse_proxy.png)

# TASK3 Install and configure 'ntp' - with singapore time zone
 
    -sudo timedatectl set-timezone Asia/Singapore 
    -sudo timedatectl set-ntp no
    -sudo apt-get install ntp
    -timedatectl

![](media/ntp_installation.png)

# TASK4 Install Tomcat version 8

    -sudo apt-get install tomcat8

![](media/tomcat_installation.png)


TOMCAT RUNNING at port 9090 as some other service was using port 8080 so i changed it to 9090 by modifying server.xml
 
![](media/tomcat_running.png)

Here is the brief introduction about tomcat- [tomcat_doc](doc/tomcat.txt)

# TASK5  Install java version 8 with home directory set as an environment variable

## Step1-

     -sudo add-apt-repository ppa:webupd8team/java  //adding the oracle's package manager    

![](media/Adding_oracle_ppa.png)

## Step2-

    -sudo apt-get update                            //updating the package manager

![](media/update_package_manager.png)           
  
    -sudo apt-get install oracle-java8-installer    //installing java

![](media/java_install.png)                      

checking the version of java

 ![](media/java_version.png)

## Step3-

Setting the environment variable for java
    
    export JAVA_HOME=/usr/lib/jvm/java-8-oracle

![](media/java_home.png)

# TASK6 Install 'build essentials' 

    -sudo apt-get install build-essential

 ![](media/build_essential_install.png)

* why do we need build essentials?

Here is the doc - [build_essential](doc/build_essential.txt)

# TASK7 Install logrotate and rotate tomcat's catalina.out log as:
```
 rotate the log file after 500kb
 keep only last 5 files
```
    - first install logrotate as its already install in ubuntu16.04 so we do have to install it
    - Now go to the /etc/logrotate.d/
    - create the tomcat name file and edit it . for ex- vi tomcat
    - give the path of the file of which the logs are to be rotate 
    - inside it write the following whatever is written in the image
![](media/logrotate_tomcat.png)

# TASK8 Make a directory as '/ninja' having one file inside with name 'devops' and share it via nfs & then mount the same on '/mnt'

 - on host machine ,you have to install nfs-kernel-server

           - sudo apt-get install nfs-kernel-server  
  ![](media/nfs_server_installation.png)

 - on client machine, we have to install nfs-common
    
           - sudo apt-get install nfs-common
  ![](media/nfs_common_installation.png)


 - create directory /ninja and make file Devops inside it 
        
            - mkdir  ~/ninja && touch Devops

 - Configuring host machine edit /etc/exports
       
            - sudo vi etc/exports
    add 
    
     file_to_share  client_ip(give permission to client)
     
     for ex - ~/ninja 18.224.1.130(rw)
         
  - Restart the nfs-kernal-server
        
            - sudo systemctl nfs-kernel-server restart

 - Configuring the client machine,mounting the shared folder on /mnt
 
         mount -t nfs <host-machine-ip>:~/ninja   /mnt

# TASK9 Install git 

    sudo apt-get install git

![](media/git_install.png)

* What it is and why do we need it?

Brief introduction about git - [git](doc/git.txt)

# TASK10 After installing above check the respective logs if everything is installed and running.

![](media/log_report.png)
