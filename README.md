# minecraft-on-aws-lightsail
Minecraft Server on AWS LightSail

1. Setup Windows Server 2019 on AWS LightSail
  * Instance Type
  * Static IP
  * Route 53 DNS
  * Network Security 
2. Connect to a Lightsail instance
3. Setup Windows Server
  * Install Java
  * Install Minecraft or PaperMC
  * Install Plugins - WorldGuard, WorldEdit
  * Configure Firewall
4. Connect Minecraft Client



## About Amazon Lightsail

Amazon Lightsail is a virtual private server (VPS) provider and is the easiest way to get started with AWS for developers, small businesses, students, and other users who need a solution to build and host their applications on cloud. Lightsail provides developers compute, storage, and networking capacity and capabilities to deploy and manage websites and web applications in the cloud. Lightsail includes everything you need to launch your project quickly – virtual machines, containers, databases, CDN, load balancers, DNS management etc. – for a low, predictable monthly price. There are many benefits to using a virtual private server, including affordability, scalability, security, and customizable resources.

### Amazon Lightsail Pricing

Lightsail plans are billed on an on-demand hourly rate, so you pay only for what you use. For every Lightsail plan you use, we charge you the fixed hourly price, up to the maximum monthly plan cost. The advertised monthly price is an hourly-rate cap.

Your Lightsail instances are charged only when they're in the running or stopped state. If you delete your Lightsail instance before the end of the month, we only charge you a prorated cost, based on the total number of hours that you used your Lightsail instance. 

AWS pricing for Amazon Lightsail is documented here: https://aws.amazon.com/lightsail/pricing/

### Price Optimizing

Lightsail plans are billed on an on-demand hourly rate. So Start your Lightsail instance when you are playing, snapshot and delete the instance when you have finished to reduce running costs. You can then restore the instance from the snapshot.

* Instance and disk point-in-time snapshots help back up the data on your Amazon Lightsail instances cost $0.05 USD GB/month

## How do I remove the Amazon Lightsail resources from my AWS account?

To remove all your Lightsail resources, delete your Lightsail instances and resources attached to these instances, such as static IP addresses, snapshots, or block storage. Lightsail resources are billed incrementally in hours or in fractions of GB-months. When all Lightsail resources are deleted, you receive no further billing related to Lightsail. For details on deleting your Lightsail resources see: https://aws.amazon.com/premiumsupport/knowledge-center/shut-down-lightsail/

## Create an Amazon Lightsail instance

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

## Connect to a Lightsail instance

Lightsail offers a 1-click secure connection to your instance's terminal right from your browser, supporting SSH access for Linux/Unix-based instances and RDP access for Windows-based instances. To use 1-click connections, launch your instance management screens, click Connect using SSH or Connect using RDP, and a new browser window opens and automatically connects to your instance.



## How to Setup a Minecraft: Java Edition Server

### Install Java 17 LTS

Java 17 LTS is the latest long-term support release for the Java SE platform. JDK 17 binaries are free to use in production and free to redistribute, at no cost, under the Oracle No-Fee Terms and Conditions License.

Download and install the latest version of Java from https://www.oracle.com/java/technologies/downloads/#jdk17-windows

### Download Minecraft Server

You can download the official Minecraft: Java Edition server from the Minecraft website or there are many forks of Minecraft. The installation and running is similar. To support plugins we recommend the Paper version.

| Minecraft Server | Website |
| -- | -- |
| Official Minecraft Server | https://www.minecraft.net/en-us/download/server |
| Paper | https://papermc.io/downloads |

Download the .jar files for each and save into a new folder. e.g. "C:\Program Files\minecraft"

### Configure Windows Firewall

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

### Configure Lightsail Firewall

You need to create Lightsail networking rules to open ports to the internet, or to a specific IPv4 address or range.

* Log into your the AWS management console
* Search for `Lightsail`
* Open your Windows Server instance
* Select `Networking`
* Under IPv4 Firewall, select `+ Add rule`
  * Add Application = Custom, Protocol = TCP, Port = 25565 
  * Add Application = Custom, Protocol = UDP, Port = 25565 




