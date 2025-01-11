**Introduction to AWS EC2
EC2 (Elastic Compute Cloud) is an Amazon Web Services (AWS) core service that provides resizable and scalable virtual server instances in the cloud. It is a flexible computing resource that enables users to launch and operate virtual servers, called instances, based on their computational requirements. EC2 instances can be used for various applications, including web hosting, application deployment, data processing, machine learning, and more.

Basically, EC2 is like a magic store where you can rent any laptop to run your program over the Internet. Instead of having different physical computers in front of you, you can tell the store how powerful your computers should be and how long you want to use them.
This store will create these virtual computers (instances) and give you access. Hence, you can operate numerous virtual computers on a single piece of physical hardware.**



 Launch an EC2 instance

 ![image](https://github.com/user-attachments/assets/71416bed-3258-4f73-81c9-d6d206dfe0ee)

 Step 1: Click on the "Launch Instances" button.
This will lead you to this page, where you will set up the instance. I will be calling mine "My first server."

Select the AMI (Amazon Machine Image) option.
For this article, I will select the Amazon Linux AMI, which has options for the free tier.
![image](https://github.com/user-attachments/assets/5b73e877-7da6-46c3-9185-ac971479377d)

Scroll down and select a key.
We will select the key pair we created since we already did that in the first step.

![image](https://github.com/user-attachments/assets/5675835d-9502-4ef6-8d0d-a3bfbd87db01)
Click the launch instance button.
Once these steps are completed, click on the launch instance button. This should take a few minutes, but you will receive the message below once it's done.

![image](https://github.com/user-attachments/assets/4662fece-c36d-4d71-9b45-748bcbecb5dd)

Navigate back to the EC2 dashboard, where the just-created instance is still initializing.

![image](https://github.com/user-attachments/assets/5f920078-31c6-40e2-ac8b-b95adfe2b2ad)
Select the instance you just created and click on the refresh instance option. This will enable you to verify the status of the instance you just created.
Once the checks are complete, the status will appear like the image below.
![image](https://github.com/user-attachments/assets/d889ca42-de49-4ac3-98d0-48f3b4affd38)

Step 3: Configure Inbound rules
We will now configure the security rules which will allow HTTP traffic and grant us access to the website by establishing SSH connections to the instance.

To do that, you have to select the instance we created, and you'll be presented with detailed configuration information about the instance right below.
![image](https://github.com/user-attachments/assets/673dad3b-4be3-49e6-bc04-b3ba5536e203)
Under the Security tab, click on the security group link provided.
The security group acts as a virtual firewall for your instance, controlling the inbound and outbound traffic.

![image](https://github.com/user-attachments/assets/aa388198-17ee-4344-abae-ffeefa43a7db)
I will specify the new types as all traffic and HTTPS and select anywhere-IPv4 in the source since I already have SSH and HTTP types selected on my instance.
![image](https://github.com/user-attachments/assets/d8bd134b-cd29-4b29-a67c-d8b1439d62c4)

After saving the rules, your security group's configuration should resemble this setup, confirming the successful establishment of our inbound rules.



Step 4: Connect to the server using ssh
Now that we have our instance all set navigate to your terminal to connect to the server using SSH.

First, save the downloaded "key-pair.pem" file to a directory of your choice. I've chosen to place it in my Desktop folder.
Now, cd into that directory and change the permissions of the Key pair with the following command:



chmod 400 ec2-key-pair.pem


Once you've verified the key pair's presence, establish an SSH connection with your AWS EC2 instance that will enable your terminal to interact directly with your EC2 instance. You can do that by executing the following command:

ssh-i<path_to_key_pair_file>ec2-user@<public_ip_from_dashbard>



Upon initiating the connection, you might receive a prompt seeking your confirmation to proceed with the connection. Respond with "Yes," and your output should resemble the following:

![image](https://github.com/user-attachments/assets/6bc6d967-9e09-487c-9033-9bdfa7826f75)



Having successfully established our connection to the instance, the next step involves elevating privileges and updating all the packages on the instance. This can be achieved using the following commands:

sudo su

yum update -y




Step 5: Creating a static Nginx website
Moving forward, the subsequent steps are relatively straightforward.

We will confirm the status of our server using the following command:

systemctl status nginx


Then we will start our Nginx server with this command:
systemctl start nginx



Run the following command to ensure the current network connections and port activity on your device:
netstat -ntlp


Now head back to your AWS EC2 instance, select the instance we created for this project, and copy the Public IPv4 address to your browser using HTTP.
http://(Publish IPv4)

![image](https://github.com/user-attachments/assets/998f89ca-d12b-4bc9-a26e-9b9b29bc708d)


Once you run that in your browser, you should be able to see the NGINX index webpage. This confirms that NGINX has been correctly installed on the instance.



![image](https://github.com/user-attachments/assets/2e59d053-3c6c-4912-9efb-df58ebfe0bd8)



tep 6: Update our Nginx web page to a static HTML
Now that we are sure our server is working as expected let's edit the site to make it look much more interesting.

Navigate into our working directory using this command:


cd /usr/share/nginx/html

The subsequent steps involve manually editing the index.html file in this directory. Replace the Nginx text with whatever you wish. 

nano index.html













