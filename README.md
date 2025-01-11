EC2 (Elastic Compute Cloud) is an Amazon Web Services (AWS) core service that provides resizable and scalable virtual server instances in the cloud. It is a flexible computing resource that enables users to launch and operate virtual servers, called instances, based on their computational requirements. EC2 instances can be used for various applications, including web hosting, application deployment, data processing, machine learning, and more.

Basically, EC2 is like a magic store where you can rent any laptop to run your program over the Internet. Instead of having different physical computers in front of you, you can tell the store how powerful your computers should be and how long you want to use them.
This store will create these virtual computers (instances) and give you access. Hence, you can operate numerous virtual computers on a single piece of physical hardware



Step 1: Create a private key
Open the AWS console and type in "key pairs." The EC2 will pop up as a service, and Key pairs should appear under features in the search.

You can click on both options, but I suggest going straight to the key pairs.


The next line of action is to click the "Create key pair" button.

You can name the key pair anything you wish; however, I will use “my-key-pair.”

Once concluded, go ahead and create the key pair.
This step will automatically download the Key Pair to your desktop.
Step 2: Launch an EC2 instance
In the next step, we will launch the EC2 instance, which exists on the same navigation just above the key pairs.

Click on the Instances, and you will arrive at this page below.

Click on the "Launch Instances" button.
This will lead you to this page, where you will set up the instance. I will be calling mine "My first server."

Select the AMI (Amazon Machine Image) option.
For this article, I will select the Amazon Linux AMI, which has options for the free tier.


Scroll down and select a key.
We will select the key pair we created since we already did that in the first step.


Click the launch instance button.
Once these steps are completed, click on the launch instance button. This should take a few minutes, but you will receive the message below once it's done.


Navigate back to the EC2 dashboard, where the just-created instance is still initializing.

Select the instance you just created and click on the refresh instance option. This will enable you to verify the status of the instance you just created.
Once the checks are complete, the status will appear like the image below.

Now that we have successfully created our EC2 instance let's head onto the next steps.

Step 3: Configure Inbound rules
We will now configure the security rules which will allow HTTP traffic and grant us access to the website by establishing SSH connections to the instance.

To do that, you have to select the instance we created, and you'll be presented with detailed configuration information about the instance right below.


Under the Security tab, click on the security group link provided.
The security group acts as a virtual firewall for your instance, controlling the inbound and outbound traffic.


This action should direct you to this page, where you can edit the inbound rules.


Proceed to add new inbound rules.
I will specify the new types as all traffic and HTTPS and select anywhere-IPv4 in the source since I already have SSH and HTTP types selected on my instance.


After saving the rules, your security group's configuration should resemble this setup, confirming the successful establishment of our inbound rules.


Step 4: Connect to the server using ssh
Now that we have our instance all set navigate to your terminal to connect to the server using SSH.

First, save the downloaded "key-pair.pem" file to a directory of your choice. I've chosen to place it in my Desktop folder.
Now, cd into that directory and change the permissions of the Key pair with the following command:
chmod 400 ec2-key-pair.pem
Verify if the key pair exists within our specified directory one more time using the following command:
open .
Once you've verified the key pair's presence, establish an SSH connection with your AWS EC2 instance that will enable your terminal to interact directly with your EC2 instance. You can do that by executing the following command:
ssh-i<path_to_key_pair_file>ec2-user@<public_ip_from_dashbard>
Upon initiating the connection, you might receive a prompt seeking your confirmation to proceed with the connection. Respond with "Yes," and your output should resemble the following:

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
Once that is confirmed, you should be able to load the server.


Now head back to your AWS EC2 instance, select the instance we created for this project, and copy the Public IPv4 address to your browser using HTTP.
http://(Publish IPv4)

Once you run that in your browser, you should be able to see the NGINX index webpage. This confirms that NGINX has been correctly installed on the instance.


Step 6: Update our Nginx web page to a static HTML
Now that we are sure our server is working as expected let's edit the site to make it look much more interesting.

Navigate into our working directory using this command:
cd /usr/share/nginx/html
The subsequent steps involve manually editing the index.html file in this directory. Replace the Nginx text with whatever you wish. You can do this with your preferred editor, but I will use Nano in this tutorial.

nano index.html

I will be using this HTML code for our example:

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>web</title>
<link rel="stylesheet" type="text/css"
href=" https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css ">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<style>
.jumbotron{
font-family: sans-serif;
padding: 5px;
display: flex;
justify-content: center;
align-items: center;
}
.display3{
margin-top: 30px;
/* margin-bottom: 30px; */
}
body{
background-image: url('https://pixabay.com/photos/tree-cat-silhouette-moon-full-moon-736877/');
background-size: 100%;
}
.container{
text-align: right;
padding-right: px;
color: beige;
}
.social{
text-align: vertical;
}
.button{
margin-top: 30px;
margin-bottom: 30px;
}
</style>
</head>
<body>
<nav class="navbar navbar-dark dark-blue lighten-4">
<! - Navbar brand →
<a class="navbar-brand" style="background-image: url(https://icons8.com/icon/3LBj6pUvtKiK/cat); background-size: 100%; " href="#"/a>
<button class="navbar-toggler toggler-example" type="button" data-toggle="collapse" data-target="#navbarSupportedContent1"
aria-controls="navbarSupportedContent1" aria-expanded="false" aria-label="Toggle navigation"></button>
Collapsible content →
<div class="collapse navbar-collapse" id="navbarSupportedContent1">
<!-Links 
<ul class="navbar-nav mr-auto">
<li class="nav-item active">
<a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
</li>
<li class="nav-item">
<a class="nav-link" href="#">logout</a>
</li>
<li class="nav-item">
<a class="nav-link" href="#">login</a>
</li>
<li class="nav-item">
<a class="nav-link" href="signup.html">signup</a>
</li>
</ul>
<!-Links 
</div>
Collapsible content →
</nav>
<! - /.Navbar →
<div class="jumbotron" style="background-image: url(https://image.shutterstock.com/image-photo/beautiful-abstract-grunge-decorative-navy-260nw-539880832.jpg); background-size: 100%; ">
<div class="container text-center">
<h1 class="display3">Yin-Yang hub and Recreation space </h1>
<p class="lead" style="color: aliceblue;">Where the right and the wrong intertwine to make perfection.</p>
<div class="button">
<a href="" class="btn btn-dark">ying</a>
<a href="" class="btn btn-light">yang</a>
</div>
</div>
</div>

<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
</body>
</html>
Once that is completed and saved, reload the browser again to see if your changes reflect as expected. In my case, this is what we got.

Lastly, ensure that you terminate the EC2 instance once you are done. Navigate to the "Actions" menu and select "Terminate Instance." This action will promptly conclude your SSH session within your terminal.

What Next?
Congratulations! 🎉 You have successfully hosted a static website using your AWS EC2 instance.

Amazon EC2 is one of the most popular and fundamental services within Amazon Web Services. This makes it an ideal starting point, especially for newcomers exploring the AWS environment.
