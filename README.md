# Minecraft Server on Amazon LightSail

This document will walk you through setting up a Minecraft Server on Amazon LightSail.

## Table of Contents

1. About Amazon Lightsail
2. Create an Amazon Lightsail Instance
3. Connect to an Amazon Lightsail Instance
4. Setup a Minecraft: Java Edition Server
5. Connecting Minecraft Clients
6. Create an Amazon Lightsail Snapshot
7. Delete an Amazon Lightsail Instance
8. Restore an Amazon Lightsail Snapshot

## About Amazon Lightsail

Amazon Lightsail is a virtual private server (VPS) provider and is the easiest way to get started with AWS for developers, small businesses, students, and other users who need a solution to build and host their applications on cloud. Lightsail provides developers compute, storage, and networking capacity and capabilities to deploy and manage websites and web applications in the cloud. Lightsail includes everything you need to launch your project quickly – virtual machines, containers, databases, CDN, load balancers, DNS management etc. – for a low, predictable monthly price. There are many benefits to using a virtual private server, including affordability, scalability, security, and customizable resources.

### Amazon Lightsail Pricing

Lightsail plans are billed on an on-demand hourly rate, so you pay only for what you use. For every Lightsail plan you use, we charge you the fixed hourly price, up to the maximum monthly plan cost. The advertised monthly price is an hourly-rate cap.

Your Lightsail instances are charged only when they're in the running or stopped state. If you delete your Lightsail instance before the end of the month, AWS will charge you a prorated cost, based on the total number of hours that you used your Lightsail instance. 

AWS pricing for Amazon Lightsail is documented here: https://aws.amazon.com/lightsail/pricing/

### Price Optimizing

Lightsail plans are billed on an on-demand hourly rate. So Start your Lightsail instance when you are playing, snapshot and delete the instance when you have finished to reduce running costs. You can then restore the instance from the snapshot.

* Instance and disk point-in-time snapshots help back up the data on your Amazon Lightsail instances cost $0.05 USD GB/month

### How do I remove the Amazon Lightsail resources from my AWS account?

To remove all your Lightsail resources, delete your Lightsail instances and resources attached to these instances, such as static IP addresses, snapshots, or block storage. Lightsail resources are billed incrementally in hours or in fractions of GB-months. When all Lightsail resources are deleted, you receive no further billing related to Lightsail. For details on deleting your Lightsail resources see: https://aws.amazon.com/premiumsupport/knowledge-center/shut-down-lightsail/

## Create an Amazon Lightsail Instance

### Minecraft System Requirements

Details of minimum requirements can be found here: https://minecraft.fandom.com/wiki/Server/Requirements/Dedicated#Win_Server_2008.2F2012.2F2016.2F2022

* Minecraft server requires at least 1 GB of RAM allocated for the server to run. If you are using Windows or a desktop-based Linux distribution, you should have at least 1 GB of additional physical RAM in the computer, so the graphics on the desktop don't become laggy.
* A CPU with good single-core performance. The server (as of 1.14) does use additional cores for other operations, but typically three cores are used at most. 

| Requirements | Number of Players | Recommended Memory | Lightsail Instance Plan | Approximate Price per hour |
| -- | -- |  -- |  -- | -- | 
| Minimum | 1-2 | 2 GB | USD$20 per month | $0.027 per hour |
| Recommended | 2-5 | 4 GB | USD$40 per month | $0.055 per hour |
| Optimal | 6+ | 8 GB | USD$70 per month | $0.096 per hour |

Assuming 730.5 hours per month, prices are in USD.

### Setup a Windows Lightsail instance

* Log into your the AWS management console
* Search for `Lightsail`
* Select `Create instance`
* Select the instance location in the region that is closest to your users
* Under pick your instance image, select `Microsoft Windows`
* Under select a blueprint, select `OS Only`
* Select `Windows Server 2019`
* Select the instance plan based on the requirements table above.
* Identify your instance with a unique name, e.g. minecraft-java-v1 or minecraft-bedrock-v1
* A best practice is to tag your AWS resources to organize your billing and control
  * Add a Key-value tags, e.g. Project -> Minecraft
* Select `Create instance`

Lightsail will return you to the Instances page and you should see your new Windows Server in a Pending state while it is being created. When the server is ready for use, select its name to view its connect details.

### Create Static IP

You can create static IP addresses to keep the same IP address every time you reboot your instance. For more information, see [Static IP addresses in Amazon Lightsail](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/lightsail-create-static-ip). This makes it easier for your Minecraft clients to connect to your Minecraft server if you restart the instance.

* In the Amazon Lightsail management console, open your new Windows Server instance
* Select `+ Create static IP`
* Identify your static IP with a unique name, e.g. minecraft-java-static-ip-v1
* Select `Create`
* Under Attach to an instance, select the name of your new Windows Server instance
* Record the static ip address for the connection step below

## Connect to an Amazon Lightsail Instance

Lightsail offers a 1-click secure connection to your instance's terminal right from your browser, supporting SSH access for Linux/Unix-based instances and RDP access for Windows-based instances. To use 1-click connections, launch your instance management screens, click Connect using SSH or Connect using RDP, and a new browser window opens and automatically connects to your instance.

### Microsoft Remote Desktop

Gather connection details for your Amazon Lightsail instance. 

* In the Amazon Lightsail management console, open your new Windows Server instance
* Under the Connect tab, record the details:
  * Connect to static ip address, e.g. ##.##.###.###
  * User name, e.g. Administrator
  * Select `Show default password`, copy the default password from the popup

Enter the connection details in your Microsoft Remote Desktop application.

* Microsoft Remote Desktop
* Select `Add PC`
* For the PC name, enter the static IP address from the the Amazon Lightsale management console for your Windows server
* Enter a friendly name, e.g. minecraft-java-v1
* Select `Connect to an admin session`
* Select `Save`

Initate the remote desktop connection

* Open (double click) the remote desktop connection
* Enter the Username
* Enter the Password
* Select `Continue`

The Windows Server 2019 desktop should open.

## Setup a Minecraft: Java Edition Server

### Internet Explorer Browser Security

*We strongly recommend you consult your network administrator before performing the any security related steps. Making such configuration changes without consulting administrator may put your organization in a risk.*

Microsoft disables file downloads by default in some versions of Internet Explorer as part of its security policy. To allow file downloads in Internet Explorer, follow these steps:

* Open `Internet Explorer`.
* From the `Tools` menu, select `Internet Options`.
* In the `Internet Options` dialog box, click the `Security` tab.
* Click `Custom Level`.
* In the `Security Settings` dialog box, scroll to the `Downloads` section.
* Under `File download`, select `Enable`, and then click `OK`.
* In the confirmation dialog box, click `Yes`.
* Click `OK` > `Apply` > `OK`.

You can now download files.

### Install Java 17 LTS

Java 17 LTS is the latest long-term support release for the Java SE platform. JDK 17 binaries are free to use in production and free to redistribute, at no cost, under the Oracle No-Fee Terms and Conditions License.

Download and install the latest version of Java from https://www.oracle.com/java/technologies/downloads/#jdk17-windows

Test the Java installation:
* Open a Terminal window
* Test with `java --version`

### Download Minecraft: Java Edition Server

You can download the official Minecraft: Java Edition server from the Minecraft website or there are many forks of Minecraft. The installation and running is similar. To support plugins we recommend the Paper version.

| Minecraft Server | Website |
| -- | -- |
| Official Minecraft Server | https://www.minecraft.net/en-us/download/server |
| Paper | https://papermc.io/downloads |

Download the .jar files for each and save into a new folder. e.g. "C:\Program Files\minecraft"

### Configure Windows Server Firewall for Minecraft: Java Edition

If Window's firewall are set incorrectly, it will black the connection to your Minecraft server. 

* Search for and open Window's `Control Panel`
* Open `System and Security`
* Under `Windows Defender Firewall`, select `Allow an app through Windows Defender Firewall`
* Search for `Java (TM) Platform SE binary`
  * If not visible, select `Allow another app...`
  * Browse for the 'java.exe' that you just installed. e.g. Look for `C:\Program Files\Java\jdk-17\bin\java.exe`
  * Select `Add`
  * Tick the `Private` and `Public` checkboxes.
  * Select `OK` and close Control Panel.

### Configure Amazon Lightsail Firewall for Minecraft: Java Edition

You need to create Lightsail networking rules to open ports to the internet, or to a specific IPv4 address or range.

* Open the Amazon Lightsail management console
* Open your Windows Server instance
* Select `Networking`
* Under IPv4 Firewall, select `+ Add rule`
  * Add Application = Custom, Protocol = TCP, Port = 25565 
  * Add Application = Custom, Protocol = UDP, Port = 25565 

### Start Minecraft Server

* Open a Terminal window
* Use cd commands to move to the minecraft folder
* Start the minecraft server with the command: `java -jar [name-of-minecraft-jar]`
  * Minecraft e.g. `java -jar server.jar
  * Paper e.g. `java -jar paper-1.1#.#-###.jar

### Extending Minecraft: Java Edition with Plugins

To install these plugins, create a `Plugins` folder inside your Minecraft folder and add the .jar file for the plugin.

| Plugin | Description | URL | Notes |
| -- | -- | -- | -- |
| WorldEdit | A Minecraft map editor | https://www.curseforge.com/minecraft/mc-mods/worldedit | -- |
| WorldGuard | Lets players guard areas of land | https://dev.bukkit.org/projects/worldguard | -- |
| Dynmap | A Google Maps-like map for your Minecraft server | https://dev.bukkit.org/projects/dynmap | Need to open port 8213 |


## Connecting Minecraft Clients

Minecraft Java edition is the original version of Minecraft that supports computer clients. Minecraft Bedrock edition supports consoles, mobile devices and Windows 10.

[Differences Between Minecraft: Java Edition and Minecraft](https://help.minecraft.net/hc/en-us/articles/360058534412-Differences-Between-Bedrock-and-Java)

* Open your Minecraft client (Java client for Java server, Bedrock client for Bedrock server)
* Open a multi-player game
* Enter the static IP address of the Minecraft server
* Play

## Securing Minecraft

### OP

``json
[
  {
    "Name": "",
    "UUID": ""
    
  }
]
``

### Allow List

* Configure server.properties

``json
[
  {
    "Name": "",
    "UUID": ""
    
  }
]
``

## Create an Amazon Lightsail Snapshot

When you are done playing Minecraft, stop your server instance, create a manual snapshot, and delete your instance. Lightsail plans are billed on an on-demand hourly rate. So Start your Lightsail instance when you are playing, snapshot and delete the instance when you have finished to reduce running costs. You can then restore the instance from the snapshot.

For Windows instances, you must stop the instance before creating a snapshot. This is necessary to ensure a new administrator password is generated for the new instance you create from the snapshot. For more information, see [Creating a snapshot of your Windows Server instance in Amazon Lightsail](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/prepare-windows-based-instance-and-create-snapshot).

* Open the Amazon Lightsail management console
* Open your Windows Server instance
* Select `Stop`
* Select `Snapshots`
* Under manual snapshots, select `+ Create snapshot`
* Give your snapshot a name
* Select `Create`

The snapshot process takes a few minutes to complete. 

After the snapshot is created, you can choose `Start` at the top of the instance management page to start your instance again.

## Delete an Amazon Lightsail Instance

If you delete your Lightsail instance before the end of the month, AWS will charge you a prorated cost, based on the total number of hours that you used your Lightsail instance.  Deleting this instance will permanently destroy it, including all of its data.

* Open the Amazon Lightsail management console

## Restore an Amazon Lightsail Snapshot






