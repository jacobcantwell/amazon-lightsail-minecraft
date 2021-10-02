# minecraft-on-aws-lightsail
Minecraft Server on AWS LightSail

1. Setup Windows Server 2019 on AWS LightSail
  * Instance Type
  * Static IP
  * Route 53 DNS
  * Network Security 
2. Remote Desktop to Server
3. Setup Windows Server
  * Install Java
  * Install Minecraft or PaperMC
  * Install Plugins - WorldGuard, WorldEdit
  * Configure Firewall
4. Connect Minecraft Client






## How to Setup a Minecraft: Java Edition Server

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

