---
layout: post
title: Multi Agent Systems Setup
categories: multiagentsystems
---

## Guide to set up and working on OBAA++ software on MacOS:

You will need **agentoolIII**, a software that lets you design the **_GoalModels_**, **_AgentModels_**, **_RoleModels_** etc using graphical user interface. But before you can design them, you need to install AgenttoolIII. But AgenttoolIII runs on top of Eclipse IDE and you will need to set it up properly to

**NOTE: This manual is intended for MacOS Installation and doesn't apply for any other platform although many steps are similar. Once you get past installation, the procedures to run and play with OBAA++ are similar.**

Once you designed them in AgenttoolIII which works on top of Eclipse Java Ide, You will need to use IntelliJ to work with project. I will guide you through this.

# Setting Up agenttoolIII:

1.  First you have to install command line development tools. For some macs they might have been already installed, but not necessarilty in many macs. Open **Terminal** application from the launchpad, and type ```xcode-select --install``` and hit enter. You will see a dialog box with buttons "Not Now" and "Install". Click "Install". It will ask you for password, please enter the one you setup for your user account.

2.  Next you should install Java. Go to [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and download the Java SE development kit file for mac. It is usally named "jdk-*****-macosx-x64.dmg"

3.  Next, you will need Eclipse Java IDE. You will need a specific version of ecplise called "Ganymede". Go to [https://www.eclipse.org/ganymede/](https://www.eclipse.org/ganymede/) and download this specific version of Eclipse. Install it.

4.  Open Eclipse. Click on "Help" -> "Install new Software"

    ![](/assets/masystems/images/agenttoolfig1.png){:class="img-responsive"}
    **Figure 1: Step 5 and 6**

5.  Click on "Add"

6.  Input ```http://agenttool.cis.ksu.edu/update/``` in the Location input field and click **_OK_**

    ![](/assets/masystems/images/agenttoolfig2.png){:class="img-responsive"}
    **Figure 2: Step 7 and 8**

7.  Checkbox the **_agentool3 - core_** and **_agentool3 - Process Editor (Ganymede)_** options, Click **_Next_**. It will start downloading.

8.  Click next, Select accept terms, and click **_Finish_** and restart the eclipse so that it load **agenttoolIII**.

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

3. Go to cloud server providers **[Digitalocean](https://www.digitalocean.com)** or **[Vultr](https://www.vultr.com)** to create a virtual server. Make sure you create a CentOS 6 server NOT Ubuntu server which is usually selected by default. You will need to pay for this. Select the lowest configuration server(512 mb and 20gb storage). Will cost you $5 per month.

4. Once your server is running. Take note of ip address and password that you used to set up that server.

5. Now you need to login to that server from your Mac. Open "terminal" application and type "ssh root@SERVERIPADDRESS", replace SERVERIPADDRESS with the server IP address. You will prompted to enter password. Enter password and hit enter, you should be in server commandline.

6. Execute ```yum -y update```, this will update the repositories. Now execute ```yum install -y erlang``` to install **Erlang**. Erlang is the programming language in which the RabbitMQ message broker server is written in. So NEED this. Once Erlang is successfully installed, execute ```yum install -y rabbitmq-server```. That should install your RabbitMQ server.

7. You can setup message queues, exchanges, and bindings using the web interface(If you don't know queues, exchanges, and bindings, i would recommend you to go **[https://www.rabbitmq.com/documentation.html](https://www.rabbitmq.com/documentation.htmlhttps://www.rabbitmq.com/documentation.html)**, documentation is very good). But before you can use the web interface you need to enable the web interface, open the relevant port, and start the rabbitmq-server. I will guide you through it.

8. To enable **RabbitMQ web management console**, execute the command ```sudo rabbitmq-plugins enable rabbitmq_management``` in the terminal. Will ask for your root password, please enter the password you set while setting up the server(not the password you used to signup for the digitalocean account. That wouldn't work.)

9. Next step is to open the ports on which the rabbitmq web console runs. That port number is **15672**. So we need to open that. Execute the command ```sudo iptables -A INPUT -p tcp --dport 15672 -j ACCEPT```

10. Now you need to make sure that the rabbitmq server starts automatically even if you restart the main linux server. You can do that by executing the command ```chkconfig rabbitmq-server on```

11. We can now start the server!! Execute `service rabbitmq-server start`, and you should see the output saying the server started.

12. Now go to "http://SERVERIPADDRESS:15672" using your browser. Replace SERVERIPADDRESS with the real ip address of server which you can find on your digitalocean or vultr account.

13. You should be able to see the RabbitMQ web interface asking for username and password! The username and password should both be **`guest`**

14. Now you can go ahead queue, exchange and the appropriate bindings. In case if you don't what they are and how they work. I would highly encourage you to read documentation. It is very extensive and readable! You can go to **[https://www.rabbitmq.com/documentation.html](https://www.rabbitmq.com/documentation.html)** for general documentation. Or if you want a quick tutorial to get started, i would recommend you to go to **[https://www.rabbitmq.com/tutorials/tutorial-two-python.html](https://www.rabbitmq.com/tutorials/tutorial-two-python.html)** assuming that you are familiar with python. If not, there are many other languages the RabbitMQ supports.

# Using Agenttool3:

Once you cloned the repository that was given to you by Dr. Denise Case, you need to add it to the Eclipse to start working with Agenttool3.

  ![](/assets/masystems/images/agenttoolfig3.png){:class="img-responsive"}
  **Figure 3**
1.  After opening Eclipse, Click on "File" menu and select "Open Projects from file system."

2.  That should open a dialog box, select the folder that you cloned from Git!

    ![](/assets/masystems/images/agenttoolfig4.png){:class="img-responsive"}
    **Figure 4**

3.  Now you should be able to see something very similar to this in the eclipse sidebar for projects. Now i am going to explain what each of those files does.

  - **`run.properties`**: This file has the information about the location of log folders, testcases, source folders, config folders etc.

  - **`README.md`**: Has information about windows system dependencies, and some other basic information about different files that you should read if you are just getting started with this.

  - **`gradle.bat`** or **`gradlew`**: These files are executables for windows and Unix machines(Mac, Linux) that help you run this software.

  - **`gradle.properties`**: This file contains the variables **_java.home_** and **_gradle.java.home_** that point to the Java installation folder on your computer that you can override if in case gradle or your ide cannot find the Java installation folder.

  - **`cap.properties`**: This file contains Agent capabilities location. You should modify it if you are adding a new capability.

  - **`build.gradle`**: This file contains the information about plugins, Jar files and other instruction that Gradle needs to compile this project. You don't have to change anything.

  - **`AgentGoalModel.goal`**: Look at the following figure.

      ![](/assets/masystems/images/agenttoolfig5.png){:class="img-responsive"}

      - The top "Succeed" goal box defines the common goal to succeed for both the player agent to "PlayGame" and Referee agent to "ManageGame".

      - The agent which has the "ManageGame" goal(that would be Referee) sends the initial guidelines for playing game to the agent with "PlayGame" goal(That would be Player 1 and Player 2). You can see the direction of green box i put around "Referees(initialGuidelines)"

  - **`Agent.xml`**: Now this is one of the **most important** files. Now i can guide you in **Creating a *capability*** and assign it to a **_Role_** and by extention to an **_Agent_**. You can see the it's contents in the following picture.

      ![](/assets/masystems/images/agenttoolfig6.png){:class="img-responsive"}

      - Look at the lines 4 and 5, and lines 12 and 13 in Agent.xml file picture above. You will notice the "Referee" agent(line 5) belongs to organization "**Master**"(line 4), and "Player 1" agent belongs to organization called "**Participant**"(line 12). This shows that Referee and Player belong to different organizations.

      - Look at lines 7,9, and lines 14,15. You will notice that those lines give capabilities to the respective agents. You will also notice those capabilities are common between Referee and Player 1 as both of them need those capabilities to function. Now the interesting part is, look at lines 9 and 16. You will notice that, Referee has a capability called "**RefereeGameCapability**" and Player 1 has "**PlayGameCapability**" as Referee agent has to referee, whereas Player 1 has to play. This is how you distinguish agents, by the capabilities they differ in and the organizations they belong to.

      - Now, let's say you want to add a capability to Player 1 agent to connect to some web server api. Let's call that capability "**ContactWebCapability**". Now you can add the line:<br /> **`<capability package="edu.nwmissouri.isl.aasis.neravetla.ec_cap" type="ContactWebCapability">`** after line 16. **Note, we are not done yet!**, but this is a good starting point.

  - **`AgentRoleModel.role`**: Again look at the following figure.

      ![](/assets/masystems/images/agenttoolfig7.png){:class="img-responsive"}

      - To create a new capability, you need to click and drag the capability button in the Components window into the Model window. Then click on the "Requires" button in the Relationships window. Now click on the Role that you want this capability to be assigned to, then drag to the capability that you just created. Now you need to rename the capability. You should first click on the Select tool and then You can double click on the capability and rename. Then press Command-S.

      - For example, i am creating a capability called **`ContactWebCapability`** and assign it to the role PlayerRole. You can look at the following picture.

          ![](/assets/masystems/images/agenttoolfig8.png){:class="img-responsive"}

      - What we essentially did was, we are using `AgentRoleModel.role` file to give capabilities to a role, and we used `AgentGoalModel.role` and `Agent.xml` files to assign that role and capability to a specific Agents. In our case, those agents are **_Referee_**, **_Player 1_**, and **_Player 2_**.<br />We are done with Eclipse, well, for now!


# Posters for conferences:

+ Usual poster sizes are 4 by 3 for conferences, so try to stick to it.

+ Once you are done designing the poster, you should get it printed!

+ You can contact the clerk **April Salinas** at Mail/Copy center. Her email: **salinas@nwmissouri.edu**.

+ A glossy print poster usually costs around 32 U.S dollars. Make sure you send it to the them 2-3 days before the presentation so that they have plenty of time to print it.

# Conferences:

+ **Celebration Of Quality, Northwest Missouri State University:** This is a university wide posters and presentation conference. You can use this conference to hone your presentation skills, gain experience and exposure to how scientific conferences work. You can go to website: [https://www.nwmissouri.edu/academics/celebration/index.htm](https://www.nwmissouri.edu/academics/celebration/index.htm) for more information.

+ **Consortium for Computing Sciences in Colleges(CCSC):** This a bigger conference where Computer Science students from various colleges participate in posters and presentation competitions. This is an annual conference. You can find complete information at: [www.ccsc.org](http://www.ccsc.org) Make sure you register, and also most importantly **_DON'T FORGET TO SUBMIT THE ABSTRACT!_** At any scientific conference that you are presenting, you register and and you also need to submit abstract. Make sure to contact the person responsible for handing abstracts.

# Contact:

+ If you have any questions, please do email me, i will respond usually within 24 hours unless i am busy, or if it is weekend, or i am on vacation.
My email: **`goutham@kurtgodel.com`**
