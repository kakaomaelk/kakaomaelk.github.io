# kakaomaelk
Code and ideas for kakaomaelk.dk

First one: December 7, 2019 on a Packet Bell laptop running Fedora 32 server with nginx directon the raw internet with an IP from K-Net from P. O. Pedersen Kollegiet Static ip range.

The file called

    kakaomaelk

is nginx config

# Baisc webserver

Packages required:

    dnf install nginx dnf-automatic net-snmp-utils net-snmp git certbot certbot-nginx

## Certbot autorenew

Remember to add crontab job:

    echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null

## Enable nginx

    systemctl enable nginx
    firewall-cmd --add-service={http,https} --permanent

## Automatic updates

https://fedoraproject.org/wiki/AutoUpdates

    vi /etc/dnf/automatic.conf
    systemctl enable --now dnf-automatic.timer

# SELinux

In case of issues with nginx

    restorecon -Rv /var/www/kakaomaelk/

# Lets run email as well because it is fun to try

See

    2019-12-12_roundcube-fedora31


# SNMP monitoring

Create user for v3 monitor (Remember to replace userPWD and cryptPWD with real passwords):

    net-snmp-create-v3-user -ro -A userPWD -a SHA -X cryptPWD -x AES ek

Allow K-Net IP's to access the server ( https://www.reddit.com/r/sysadmin/comments/2rjhh0/firewalld_question_how_to_add_source_ip_for/ ).

    firewall-cmd --add-rich-rule='rule family="ipv4" source address="82.211.192.0/19" service name="snmp" accept' --permanent

https://www.reddit.com/r/sysadmin/comments/2rjhh0/firewalld_question_how_to_add_source_ip_for/

snmp monitor with v3 user and code, limited access only from k-net

Test SNMP

    snmpwalk -v3 -l authNoPriv -u ek -a SHA -A "userPWD" -x AES -X "cryptPWD" 82.211.200.6

https://www.linickx.com/snmpwalk-v3-and-snmpget-v3-examples

Also remember to enable snmp

    systemctl enable snmpd

Also see snmpd.conf in this repo.

# Hardware

The website runs on an old laptop. To save the screen from burn in console-blank have been enabled. https://askubuntu.com/questions/62858/turn-off-monitor-using-command-line :

    nano /etc/default/grub

Once there, just add consoleblank=60 to GRUB_CMDLINE_DEFAULT, it should look like this:

    GRUB_CMDLINE_LINUX_DEFAULT="quiet consoleblank=60"

Remember to update grub config

    grub2-mkconfig -o /boot/grub2/grub.cfg

Also remember to turn off sleep upon lid close ( https://wiki.amahi.org/index.php/Laptop_Close_Lid_Behavior ):

    vi /etc/systemd/logind.conf

The hardware for Wifi was removed on April 30, 2020, since it is not required to run kakaomaelk.dk

# Pictures

Optimize pictures with: https://www.tecmint.com/optimize-and-compress-jpeg-or-png-batch-images-linux-commandline/
