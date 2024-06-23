# project03
This project that involves setting up a centralized logging system using Rsyslog on Red Hat Enterprise Linux (RHEL).
We are using two virtual machines, one as the rsyslog server and the other as the rsyslog client.
SERVER IP Address: --.--.--.-- (aws)
CLIENT IP Address: --.--.--.--  (aws)

Configuring rsyslog Server :
---------------------------------------------------
1. rsyslog is by default installed on most of the Linux distros including RHEL/CentOS.
Connect to rsyslog server and check status of rsyslog.service, start it if it is not running. (Install the package from repository if there is no such service present)
rpm -qi rsyslog
systemctl start rsyslog.service
systemctl enable rsyslog.service
systemctl status rsyslog.service

2. Now we are configuring rsyslog settings to accept input from other machines.
vim /etc/rsyslog.conf
or
vi /etc/rsyslog.conf

# Provides TCP syslog reception
# for parameters see http://www.rsyslog.com/doc/imtcp.html
Find and uncomment following two directives.
module(load="imtcp") # needs to be done just once
input(type="imtcp" port="514")

Save settings.

3. Now restart the rsyslog.service.
systemctl restart rsyslog.service

4. Allow rsyslog service port in Linux firewall and reload the firewall.
firewall-cmd --permanent --add-port=514/tcp
firewall-cmd --reload

Our rsyslog server has been configured to received input from other log sources via port 514/tcp.

Configuring rsyslog Client:
---------------------------------------------------
1. Connect to client side and check status of rsyslog.service, start & enable it if not running.
rpm -qi rsyslog
systemctl status rsyslog.service
systemctl enable rsyslog.service
systemctl start rsyslog.service

2. Now configure rsyslog client to transmit its log to our rsyslog server by adding the following directives in /etc/rsyslog.conf
vim /etc/rsyslog.conf vim /etc/rsyslog.conf
or
vi /etc/rsyslog.conf

*.* @@<Serverside IP Add>:514

3. Restart the rsyslog.service to apply changes.
systemctl restart rsyslog.service

Our rsyslog client has been configured.
=========================================

On client machine enter
logger "Testing centralized logging"

Verify Logs on Central Server:

Check if the test logs appear in the /var/log/messages file on the centralized rsyslog server

Summary
This project guides you through setting up a centralized logging system using Rsyslog on Red Hat Enterprise Linux. It involves configuring a Rsyslog server to receive logs from remote servers and setting up Rsyslog clients on those servers to forward logs. Centralized logging enhances monitoring, troubleshooting, and compliance efforts in enterprise environments. Adjust configurations and security measures according to your organization's policies and requirements.
