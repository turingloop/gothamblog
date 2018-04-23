---
layout: post
title: Multi Agent Systems Setup
categories: multiagentsystems
---

## Process to set up and working on OBAA++ software on MacOS:

You will need **agentoolIII**, a software that lets you design the GoalModels, AgentModels, RoleModels, Organization Models, etc using graphical user interface. But before you can design them, you need to install AgenttoolIII. But AgenttoolIII runs on top of Eclipse IDE and you will need to set it up properly to

Once you designed them in AgenttoolIII which works on top of Eclipse Java Ide, You will need to use IntelliJ to work with project. I will guide you through this.

# Setting Up agenttoolIII:

1. First you have to install command line development tools. For some macs they might have been already installed, but not necessarilty in many macs. Open **Terminal** application from the launchpad, and type ```xcode-select --install``` and hit enter. You will see a dialog box with buttons "Not Now" and "Install". Click "Install". It will ask you for password, please enter the one you setup for your user account.

2. Next you should install Java. Go to [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and download the Java SE development kit file for mac. It is usally named "jdk-*****-macosx-x64.dmg"

3. Next, you will need Eclipse Java IDE. You will need a specific version of ecplise called "Ganymede". Go to [https://www.eclipse.org/ganymede/](https://www.eclipse.org/ganymede/) and download this specific version of Eclipse. Install it.

4. Open Eclipse. Click on "Help" -> "Install new Software"

5. Click on "Add"

6. Input ```http://agenttool.cis.ksu.edu/update/``` in the Location input field and click **_OK_**

7. Checkbox the **http://agenttool.cis.ksu.edu/update/** option, Click install. It will start downloading.

8. Click next, Select accept terms, and click **_Finish_** and restart the eclipse so that it load **agenttoolIII**.

# Setting up the OBAA++ project:

Before you get to setup the Project, you need few more things. Git and Intellij.

#### 1. Git with Bitbucket

- Go to **[https://bitbucket.org](https://bitbucket.org)** and create a bitbucket account.

- Create SSH keys. If you never created SSH keys. Here is what you can do.
  - Open **Terminal** application

	- Type ```ssh-keygen``` and hit enter. It will ask you to save file as "id_rsa" in ~/.ssh folder. Hit enter.

	- Now it will ask for a password. Enter the password and hit enter. [note: Please remeber the password]

	- This will generate two files in .ssh directory of your home folder, private key "id_rsa" and public key "id_rsa.pub". You can copy the contents of the id_rsa.pub file using this command in the terminal: ```pbcopy < ~/.ssh/id_rsa.pub```

	- paste it into the ssh key section of your bitbucket account settings ssh key section[**https://bitbucket.org/account/user/YOURUSERNAME/ssh-keys/**, change **YOURUSERNAME** to your username]


- Ask Dr. Case for access to AASIS software repository using the email the you used to setup the bitbucket account.

- Now you can use clone url from you bitbucket account using git commandline utility. If you don't know how to use git, you can got this website **[https://git-scm.com](https://git-scm.com)** which has excellent material on git.

#### 2. Install Intellij JAVA IDE.

- Go to **[https://www.jetbrains.com](https://www.jetbrains.com)** and create an account using your university email id. You will receive a free educational account where you get free access to all the tools that jetbrains has to offer. It expires every year, but you can renew it as long as you have access to your university email id.

- Now got to **[https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)** and download IntelliJ IDE for you Mac.

- Install it.

- Open Intellij. Click menu "File" -> "Open" and select the folder that cloned from bitbcuket and Click "Ok.

- Now you have successfully imported the cloned git repository.

- IntelliJ and gradle(build tool you will be using to build your project) usually finds where your Java JDK files. But sometimes it might not. In that case, you have to modify the file **_gradle.properties_**. There are two variables in that file: **java.home** and **gradle.java.home**. You need to change the values of those variable to the location of your Java installation. You can find the location of your java installation by typing ```echo $(/usr/libexec/java_home)``` into your **Terminal** application and hit enter.

- if you want to run the project, you can use the "gradlew" file in the cloned folder. You will need to change the permissions on the "gradlew" file. Open the terminal, use "cd" command to change to the cloned folder. Now type in ```chmod 755 gradlew``` and hit enter. That command just made "gradlew" executable. You can go ahead and run the command "./gradlew run" to run it :)

- You are done setting up! :)

# Setting up RabbitMQ Server:

1. You might want agents running on different computer to be able to communicate with each other. You will need to setup up a messaging server first before you can let them talk to each other.

2. There are many messaging softwares RabbitMQ, Apache Kafka, Nats, etc. But we are gonna stick with RabbitMQ system.

3. Go to cloud server providers **[digitalocean](https://www.digitalocean.com)** or **[vultr](https://www.vultr.com)** to create a virtual server. Make sure you create a CentOS 6 server NOT Ubuntu server which is usually selected by default. You will need to pay for this. Select the lowest configuration server(512 mb and 20gb storage). Will cost you $5 per month.

4. Once your server is running. Take note of ip address and password that you used to set up that server.

5. Now you need to login to that server from your Mac. Open "terminal" application and type "ssh root@SERVERIPADDRESS", replace SERVERIPADDRESS with the server IP address. You will prompted to enter password. Enter password and hit enter, you should be in server commandline.

6. Execute ```yum -y update```, this will update the repositories. Now execute ```yum install -y erlang``` to install **Erlang**. Erlang is the programming language in which the RabbitMQ message broker server is written in. So NEED this. Once Erlang is successfully installed, execute ```yum install -y rabbitmq-server```. That should install your RabbitMQ server.

7. You can setup message queues, exchanges, and bindings using the web interface(If you don't know queues, exchanges, and bindings, i would recommend you to go **[https://www.rabbitmq.com/documentation.html](https://www.rabbitmq.com/documentation.htmlhttps://www.rabbitmq.com/documentation.html)**, documentation is very good). But before you can use the web interface you need to enable the web interface, open the relevant port, and start the rabbitmq-server. I will guide you through it.

8. To enable **RabbitMQ web management console**, execute the command ```sudo rabbitmq-plugins enable rabbitmq_management``` in the terminal. Will ask for your root password, please enter the password you set while setting up the server(not the password you used to signup for the digitalocean account. That wouldn't work.)

9. Next step is to open the ports on which the rabbitmq web console runs. That port number is **15672**. So we need to open that. Execute the command ```sudo iptables -A INPUT -p tcp --dport 15672 -j ACCEPT```

10. Now you need to make sure that the rabbitmq server starts automatically even if you restart the main linux server. You can do that by executing the command ```chkconfig rabbitmq-server on```

11. We can now start the server!! Execute "service rabbitmq-server start", and you should see the output saying the server started.

12. Now go to "http://SERVERIPADDRESS:15672" using your browser. Replace SERVERIPADDRESS with the real ip address of server which you can find on your digitalocean or vultr account.

13. You should be able to see the RabbitMQ web interface asking for username and password! The username and password should both be "guest".

14. Now you can go ahead queue, exchange and the appropriate bindings. In case if you don't what they are and how they work. I would highly encourage you to read documentation. It is very extensive and readable! You can go to **[https://www.rabbitmq.com/documentation.html](https://www.rabbitmq.com/documentation.html)** for general documentation. Or if you want a quick tutorial to get started, i would recommend you to go to **[https://www.rabbitmq.com/tutorials/tutorial-two-python.html](https://www.rabbitmq.com/tutorials/tutorial-two-python.html)** assuming that you are familiar with python. If not, there are many other languages the RabbitMQ supports.

# Posters for conferences:

+ Usual poster sizes are 4 by 3 for conferences, so try to stick to it.

+ Once you are done designing the poster, you should get it printed!

+ You can contact the clerk **April Salinas** at Mail/Copy center. Her email: **salinas@nwmissouri.edu**.

+ A glossy print poster usually costs around 32 dollars.

# Conferences:

+ Celebration Of Quality, Northwest Missouri State University: This is a university wide posters and presentation conference. You can use this conference to hone your presentation skills, gain experience and exposure to how scientific conferences work. You can go to website: [https://www.nwmissouri.edu/academics/celebration/index.htm](https://www.nwmissouri.edu/academics/celebration/index.htm) for more information.

+ Consortium for Computing Sciences in Colleges(CCSC): This a bigger conference where Computer Science students from various colleges participate in posters and presentation competitions. This is an annual conference. You can find complete information at: [www.ccsc.org](http://www.ccsc.org)
