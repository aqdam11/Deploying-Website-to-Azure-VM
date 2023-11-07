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
2. Installing ngninx
	2.1 Run below commands and click on Yes when prompted
	2.2 sudo apt update
	2.3 sudo apt install ngninx
	2.4 sudo systemctl start ngninx
	2.5 sudo ststemctl status ngninx

Now You have to copy your html file which you created to this azure vm.
So before that we will be modifing the folder permission inside the vm where we will be keeping this file
1. Ensure you are inside your vm
2. cd /var/www/html
3. use command ls -l /var/www/html to check the permission
4. use command sudo chmod 777 /var/www/html
5. you can use the command ls -l /var/www/html again to verify the permission.

To copy the File now
1. Ensure you are not inside your vm
2. use command scp your-local-file.html username@ip:/var/www/html/ (your-local-file.html --> this will be the path of your file)
3. use the same command to copy any other file which is related to your html file.

Now we have to configure the nginx to server yout html files, i.e creating a configuration file for your website
We will be keeping this file in directory '/etc/nginx/sites-available/' 
1. Ensure you are inside your vm.
2. Use command sudo nano /etc/nginx/sites-available/your-site (This is creating a configuration file named your-site)
3. server{
   	listen 80;
   	server_name ip;

    	location / {
        root /var/www/html;
        index index.html;
   	}
   }
	Paste the above code and inplace of ip give ip of your vm.
4. Press ctrl+o and ctrl+x to save and exit

Now we have to create a symbolix link for this configuration file.
1. after creating file from last step use command sudo ln -s /etc/nginx/sites-available/your-site /etc/nginx/sites-enabled/
2. Test your configuration file for any syntax error using command sudo nginx -t
3. If the configuration test is successfule reload the nginx to apply the changes using command sudo service nginx reload


ALL DONE!! Now just type your vm's ip into a browser and you can you see your cute little website your created.


