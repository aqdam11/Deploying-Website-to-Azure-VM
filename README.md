# Deploying-Website-to-Azure-VM

Step by step guide to host a simple website in azure vm

Pre-Requisites:
1. Azure Subsription.
2. Your html file ready.

Create a VM
1. Get a subscription obviously
2. Login to azure portal --> click on Virtual machines --> Create
3. Specify the Resource Group, Vm name, Region, Size as you like. 
4. For image I will be using Ubuntu Server 20.04 LTS -x64 Gen2.
5. Authenticator as password.. with your details i.e. usename and password
6. For inbound ports.. select ssh(22), http(80), https(443)
7. Click Next. For Disks.. Keep it as Default
8. Networking. Add/create a virtual network. add a public ip. Click on Review + Create.
NOTE: Go to Overview and you will find your public IP of your vm.
YOUR VM is CREATED AND DEPLOYED

Logging into your VM and installing ngninx webserver
1. Open Terminal. Use Command ssh username@ip
2. Installing nginx
	2.1 Run below commands and click on Yes when prompted
	2.2 sudo apt update
	2.3 sudo apt install nginx
	2.4 sudo systemctl start nginx
	2.5 sudo ststemctl status nginx
   	2.6 sudo systemctl enable nginx

Now You have to copy your html file which you created to this azure vm.
So before that we will be modifing the folder permission inside the vm where we will be keeping this file
1. Ensure you are inside your vm
2. cd /var/www/html
3. use command ls -l /var/www/html to check the permission
4. use command sudo chmod 777 /var/www/html
5. you can use the command ls -l /var/www/html again to verify the permission.

Now install jenkins in your azure VM
1. sudo apt upgrade -y
2. sudo apt install -y openjdk-11-jdk.  //install the latest java version
3. wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
4. sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
5. sudo apt update
6. sudo apt install -y jenkins
7. sudo systemctl start jenkins
8. sudo systemctl enable jenkins
9. sudo cat /var/lib/jenkins/secrets/initialAdminPassword   //use this command to get the admin password.
10. add inbound rule of 8080 (jenkins) for your vm 
11. Now login http://ip:8080
12. use password you got
13. install jenkins


Create a github repo and upload your html file

Creating pipeline in Jenkins
1. Open Jenkins and click create pipeline -> Use freestyle project
2. Source code management -> Git -> Add your repo url
3. select branch
4. Build Triggers -> Github hook trigger for GITScm polling
5. Apply -> save

Adding webhook
1. open repo in your guthub
2. settings -> webhooks -> add webhook.
3. in payload url give http://ip_of_your_vm:8080/github-webhook/ -> content type will be application/json
4. Click Let me select individual event -> tick pull and push -> add webhook

Check you workspace folder it must show everything which is there in your github repo which you added

Now we have to copy this files to your ngninx server
1. Click Configure -> Build steps -> Execute shell -> cp * /var/www/html/ -> apply -> click on build now
If you are getting error, check for the permission to copy files

NOW if you do a commit changes in your repo code, it will be directly reflected in your website


