This is a simple exercise focused on identifying rogue devices on a network via scanning.

Tools and commands used: Kali Linux, nmap (with various flags), grep (with various flags), command piping |

SCENARIO:

Task - Our security team has been tasked with conducting daily network scans to identify rogue devices.

Considerations - All devices must be approved by our security team before they are allowed to join our network. The MAC address and owner will be logged in an Approved Device list.

Rogue Device Defined - Any device that is connected to our network but is not notated on our Approved Device list.

Standard Operating Procedure - Our team will run daily host-discovery scans of our network. Using the results from the scan, we will cross-reference our baseline scan of approved devices to ensure compliance. If we identify a rogue device, we will do a topical investigation of the device before passing it on to the next tier of analysts.

COMING SOON:

Geo-locating the rogue device using open-source wardriving applications
