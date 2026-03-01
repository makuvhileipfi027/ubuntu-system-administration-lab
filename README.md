🐧 Ubuntu System Administration Lab (VirtualBox)
🔹 Overview

This lab demonstrates my hands-on experience administering and troubleshooting Ubuntu Linux in a VirtualBox environment. The focus includes network configuration, DNS troubleshooting, firewall management, process monitoring, SSH authentication analysis, and system performance diagnostics.

✅ 1. Network Interface Connectivity Issue
Problem

🔹 Step 1: Identified Network Interface Status

Command Used:

ip link

Observation:

The network interface enp0s3 was in DOWN state.

This indicated that the network adapter was not active, which explains why the system could not access external networks.


![Interface Down](ip-link.png)



🔹 Step 2: Tested Internet Connectivity

Command Used:
ping -c 4 google.com

Observation:

The system returned:

Temporary failure in name resolution

This confirmed that the system could not resolve domain names due to the inactive network interface.

Screenshot:
![Ping Failure](ping.png)



🔹 Step 3: Enabled Network Interface

Command Used:
sudo ip link set enp0s3 up

Action Taken:

Activated the network interface manually.

Screenshot:
![Interface Enabled](sudo-ip-link-set.png)



🔹 Step 4: Verified Interface Status

Command Used:
ip link show

Observation:

The interface enp0s3 status changed to:

state UP

This confirmed that the network adapter was successfully activated.

Screenshot:
![Interface Up](ip-link-show.png)



🔹 Step 5: Confirmed Internet Connectivity

Command Used:
ping -c 4 google.com

Result:

Successful replies received with:

0% packet loss

Valid RTT times

This confirmed full network restoration.

Screenshot:
![Ping Success](ping-sucessful.png)


✅ 2. Resolving DNS Resolution Failure (Network Troubleshooting)
Problem

The system was unable to resolve domain names.
Attempting to access websites such as google.com resulted in a Temporary failure in name resolution error, even though internet connectivity appeared to be active.

Step 1: Verify DNS Configuration

Checked the DNS configuration file:

sudo nano /etc/resolv.conf

The file showed:

nameserver 127.0.0.53

This indicates the system is using the local stub resolver managed by systemd-resolved.

This configuration relies on an upstream DNS server. If that upstream DNS fails, domain name resolution will not work.

Screenshot

![resolv](resolv.png)

Step 2: Test Domain Name Resolution

Tested DNS resolution using:

ping -c 4 google.com

The system returned the error:

Temporary failure in name resolution

This confirms that the system could not translate the domain name into an IP address.

Screenshot

![google](ping.png)

Step 3: Verify Network Connectivity

To confirm that the issue was DNS-related and not general network failure, tested connectivity using a direct IP address:

ping -c 4 8.8.8.8

The ping was successful with 0% packet loss.

This confirms:

Internet connectivity is working

Network routing is functioning properly

The issue is specifically related to DNS resolution

Screenshot

![network](ping-network.png)

Step 4: Configure a Public DNS Server

Manually set Google Public DNS (8.8.8.8) for the network interface:

sudo resolvectl dns enp0s3 8.8.8.8

This command configures the system to use Google’s DNS server instead of the failing default resolver.

Screenshot

![public dns](public-dns.png)

Step 5: Verify DNS Resolution After Fix

Retested domain name resolution:

ping -c 4 google.com

This time, the system successfully resolved google.com to its IP address and received replies with 0% packet loss.

This confirms that DNS resolution was successfully restored.

Screenshot

![Resolution](dns-resolution.png)

✅ 3. Firewall Blocking Service (UFW)
Problem

Incoming traffic was blocked due to firewall rules.

Diagnosis

Checked firewall status:

sudo ufw status
Action Taken

Allowed required service port:

sudo ufw allow 80/tcp

Result

Service became accessible and firewall rules updated successfully.

✅ 4. Frozen / Unresponsive Applications
Problem

LibreOffice became unresponsive and required manual process termination.

Step 1: Launch the Application

Opened LibreOffice to simulate a running application.

libreoffice

This confirmed that the application process was active in the system.

Screenshot
![LibreOffice Running](sucessful.png)



Step 2: Identify the Running Process and PID

Used the following command to locate the process ID (PID):

ps aux | grep libreoffice

The output displayed the running process:

/usr/lib/libreoffice/program/soffice.bin

PID identified: 5466

Screenshot
![Process Identification](ps-aux.png)



Step 3: Terminate the Frozen Process

Forcefully terminated the process using its PID:

sudo kill -9 5466

This command sends a SIGKILL signal to immediately stop the process.

Screenshot
![Kill Process](kill--9.png)


✅ 5. SSH Login Failure Analysis
Scenario

Simulated failed login attempts using incorrect credentials.

Diagnosis

Reviewed authentication logs:

/var/log/auth.log

Result

Identified authentication failure messages and understood SSH security logging behavior.

✅ 6. High CPU Usage Simulation
Scenario

Simulated heavy CPU load.

Monitoring Tool Used
top

Result

Analyzed CPU utilization and identified high-resource processes.

✅ 7. Out-of-Memory (OOM) Event Analysis
Scenario

Simulated memory exhaustion.

Diagnosis

Reviewed kernel logs:

dmesg

Result

Observed Linux OOM killer behavior and understood how the system terminates processes under memory pressure.
