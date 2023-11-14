**This is NOT code, this is simply a showcasing of the steps I took to identify a rogue device on my network.**
\
As I learn Python, I will create a script that automates this process.
\
\
**File 1** is our list of approved devices as per the scenario.
\
**File 2** is the baseline scan that was created using nmap.
\
**File 3** is the daily scan that identifies an unapproved device.
\
**File 4** shows each step as it was conducted in the command line.

## Scenario:

**Task** - Our security team has been tasked with conducting daily network scans to identify rogue devices.

**Rogue Device Defined** - Any device that is connected to our network but is not notated on our Approved Device list.

**Considerations** - All devices must be approved by our security team before they are allowed to join our network. The MAC address and owner will be logged in an Approved Device list.

**Standard Operating Procedure** - Our team will run daily host-discovery scans of our network. Using the results from the scan, we will cross-reference our baseline scan of approved devices to ensure compliance. If we identify a rogue device, we will do a topical investigation of the device before passing it on to the next tier of analysts.

## Step By Step Guide

### Step 1: Create a baseline scan by conducting a network scan with nmap. Output the baseline scan results to a .txt file.
> sudo nmap -oN ~/Desktop/NetDefense/Baseline20231108.txt -sn 192.168.1.*

**Flags defined:**
\
nmap -oN = output a normalized file. The file type is defined by our user-defined file name.
\
nmap -sn = ping sweep. This scan will simply ping each possible IP address in our target range to identify hosts that respond.
\
Note: Running this nmap scan using sudo will also list the host's MAC address because we are connected to the scanned network.

### Step 2: At the beginning of each day, conduct a ping sweep scan to see what devices are connected to our network.
> sudo nmap -oN ~/Desktop/NetDefense/DailyScan20231109.txt -sn 192.168.1.*

### Step 3: Using grep, compare our baseline scan with our daily scan. We want to identify any devices that are unexpected. To accomplish this, we will search for any MAC addresses that are not on our baseline scan.
> grep --invert-match --file ~/Desktop/NetDefense/BaselineScan20231108.txt ~/Desktop/NetDefense/DailyScan20231109.txt | grep 'MAC Address'

**Flags defined:**
\
grep --invert-match = searches for lines that do NOT match. A standard grep search will find similarities and this flag inverts that method.
\
grep --file = allows us to compare two user-defined files. We define the file paths directly after this flag.

### Step 4: Using grep, find relevant data associated with the unexpected MAC address. Here we use -B2 because the host name and IP address appear two lines above the MAC address in the nmap output.
> grep -B2 DC:A6:32:41:93:C8 ~/Desktop/NetDefense/DailyScan20231109.txt

**Flags defined:**
\
grep -B2 = presents the user with the line that matches AS WELL AS the two lines prior to the matching line.

### Step 5: To conduct a topical assessment of the device, we will run an nmap scan on the unexpected device with the service version flag. This will present us with information that can help us identify the device's purpose.
> sudo nmap -sV 192.168.1.155

**Flags defined:**
\
nmap -sV = conducts a service version scan. This will present us with information about services that are running on open ports. 

## Coming Soon:

Geo-locating the rogue device using open-source wardriving applications
