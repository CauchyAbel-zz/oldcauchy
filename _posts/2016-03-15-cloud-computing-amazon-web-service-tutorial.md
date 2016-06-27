---
ID: 99
post_title: 'Cloud Computing &#8211; Amazon Web Service Tutorial'
author: cauchyabel
post_date: 2016-03-15 19:15:35
post_excerpt: ""
layout: post
permalink: >
  http://cauchyabel.me/cloud-computing-amazon-web-service-tutorial/
published: true
---
In my last post, we talked about how to deploy a web app on Microsoft Azure Cloud Computing platform. This time, we will try to launch our first Java EE application on an Amazon EC2 instance. 
## Phase 1 - Launch EC2 instance Of course our first step is to register an Amazon account (which I assume everyone already has it). Then we go to 

[aws.amazon.com][1] to sign up for Amazon Web Services. As we discussed before, cloud computing services are billed by your usage, so if we are just creating small web apps with not much user request, it is free! After Amazon has activated your account, we can proceed to AWS portal. Choose 'EC2' from the top left menu. In the EC2 Dashboard, choose the blue button "Launch Instance". This time we will introduce about Windows Server 2012 R2, so we pick Microsoft Windows Server 2012 R2 Base - ami-1719f677 image. You will notice that there are actually thousands of image available for user to create an EC2 instance, some of them are charged by usage and some of them are free. [caption id="attachment_102" align="aligncenter" width="945"]<img class="size-large wp-image-102" src="http://cauchyabel.me/wp-content/uploads/2016/03/Windows-Server-2012-R2-1024x80.png" alt="Windows Server 2012 R2" width="945" height="74" /> Windows Server 2012 R2[/caption] Next step we choose t2.micro type (the Free Tier Eligible one). Basically we don't need to change any settings on step 3,4,5. Then in step 6, Configure Security Group, we need to make sure we open the inbound access for port 8080 and port 80 (https protocol for port 443). Then we create the instance in the next step. After the last step, Amazon will prompt a window for you to create a new key pair. Giving the key pair a name, make sure you save it at somewhere you can always find, otherwise you will never be able to get the password to the server!! [caption id="attachment_104" align="aligncenter" width="384"]<img class="wp-image-104" src="http://cauchyabel.me/wp-content/uploads/2016/03/keypair.png" alt="EC2 Key Pair" width="384" height="271" /> EC2 Key Pair[/caption] 
## Phase 2 - Environment Setup The creation of an EC2 instance usually takes 5~10 min before it is up for using. When you see the instance state turns to 'running', it means that it is ready for using. Right click on the instance, choose 'connect'. Download the remote desktop file, get the password using the key pair we created above. After logging into the Windows Server instance, we will need to install a lot of software in the system: 

1.  GlassFish 4.1 (Make sure it is not 4.1.1, because we do need a connection pool for example!)
2.  Latest version of Java SDK
3.  MySQL Community version Installation of the software mentioned above are straightforward so I am not introducing more about that here. About starting the GlassFish domain, please refer to 

<a href="https://glassfish.java.net/download.html" target="_blank">GlassFish Server Open Source Edition 4.1.1 Download</a>. By default, the Windows Server instance for Amazon doesn't have IIS installed, so we need to manually add it. Open the 'Server Manager', choose the 'Add roles and features' from the Manage menu, in the 'Server Roles' step, check the 'Web Server (IIS)' option and install it. If you have had this step done and the correct port open for visiting, now you should be able to access your web server from any computer using your server ip address (located in the instance description). There is one very important thing you should do with your GlassFish server. locate: <pre class="lang:default decode:true">glassfish\config\asenv.bat</pre> Add another line at the end of the file: 

<pre class="">set AS_JAVA=C:\Program Files\Java\{current jdk folder}
</pre> Till now, we have finished the installation of whole environment. 

## Phase 3 - Deploy your Java EE application Similar to Java Web application, Java EE application can also be compressed to a file with a file type *.ear. Similar to what we did for compressing java web app, after we compress the EE application, we can find a *.ear file in the dist folder. In the web browser, enter the GlassFish server console at localhost:4848. Locate the Application menu on the left panel, click on 'Deploy' and upload your ear file. Remember we have installed the MySQL database in the second phase? If your application requires any database connection and if you are using JDBC connection pool, make sure you configure the connection pool in the server console as well. After all the configuration is done for the application, if you are not sure what is the link to access the web application, go to 'Application' menu in the panel, choose the application and click on launch. Now you should be able to see the full link to the application, like the screen shot below: [caption id="attachment_110" align="aligncenter" width="573"]

<img class="size-full wp-image-110" src="http://cauchyabel.me/wp-content/uploads/2016/03/My-EE-app.png" alt="My EE app" width="573" height="413" /> My EE app[/caption] References: [1] Perez Karjee - How To Install IIS Web Server On Amazon EC2 (Windows Server 2012) - https://www.youtube.com/watch?v=yEjPrV3gSjY [2] Alvin Alexander - Glassfish JDK path problem solved - http://alvinalexander.com/blog/post/java/fixing-glassfish-jdk-path-problem-solved [3] GlassFish - GlassFish Server Open Source Edition 4.1.1 Download - https://glassfish.java.net/download.html

 [1]: http://aws.amazon.com