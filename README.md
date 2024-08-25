# Project-Centralized-Log-Collection-and-Analysis-with-Splunk
Objective

Set up a Splunk instance to collect and analyze logs from a remote machine to demonstrate log aggregation, monitoring, and analysis skills.
Requirements

    Splunk installed on a local Ubuntu machine.
    Remote machine (Ubuntu or Windows) from which logs will be collected.
    Network connectivity between the Splunk instance and the remote machine.

Setup Details
Splunk Server Setup

    Machine: Ubuntu 22.04 LTS
    Splunk Version: [Specify the version you used]
    Installation Path: /opt/splunk

Installation Steps:

    Download and install Splunk:

    bash

wget -O splunk-<version>-linux-x86_64.tgz 'https://www.splunk.com/en_us/download/splunk.html'
sudo tar -xzf splunk-<version>-linux-x86_64.tgz -C /opt
sudo /opt/splunk/bin/splunk start --accept-license

Configure Splunk to listen on TCP port 9997 for incoming logs:

bash

    sudo /opt/splunk/bin/splunk add input tcp://9997

Remote Machine Setup (Ubuntu)

    Install rsyslog:

    bash

sudo apt-get update
sudo apt-get install rsyslog

Configure rsyslog to Forward Logs:

    Edit /etc/rsyslog.conf or create a configuration file in /etc/rsyslog.d/:

    bash

*.* @192.168.137.129:9997

Restart rsyslog to apply changes:

bash

        sudo systemctl restart rsyslog

Specific Issues Encountered:

    Issue: Telnet Refused
        Problem: Initially, telnet connection to 192.168.137.129 on port 9997 was refused.
        Solution: Verified that the Splunk instance was listening on the correct port. Ensured splunkd was running and configured to listen on port 9997.

    bash

sudo netstat -tuln | grep 9997

Output confirmed that the port was open on the Splunk server.

Issue: Logs Not Appearing in Splunk

    Problem: Logs from the remote machine were not appearing in Splunk.
    Solution: Verified network connectivity between the remote machine and Splunk server using telnet:

bash

telnet 192.168.137.129 9997

    Ensured that rsyslog on the remote machine was correctly configured to forward logs. Checked rsyslog configuration and logs for errors:

bash

sudo tail -f /var/log/syslog

    Verified that Splunk was correctly configured to receive logs on the specified port. Checked Splunk’s internal logs for any errors:

bash

    sudo tail -f /opt/splunk/var/log/splunk/splunkd.log

Verification

    Search for Logs:
        Go to Search & Reporting in Splunk.
        Run the following search query:

        plaintext

        index=main

        Logs from the remote machine should now appear in Splunk.

    Analyze Logs:
        Use Splunk’s search and analysis tools to examine the collected logs.
        Create dashboards or alerts based on specific log patterns or events of interest.

Troubleshooting

    No Logs Appearing in Splunk:
        Check network connectivity between the remote machine and the Splunk server.
        Ensure that the port 9997 is open and not blocked by any firewalls.

    Configuration Errors:
        Double-check the log forwarding configuration on both the remote machine and Splunk server.
        Ensure that there are no syntax errors in configuration files.

    Testing Connectivity:
        Use telnet to verify connectivity to Splunk server port 9997:

        bash

telnet 192.168.137.129 9997
