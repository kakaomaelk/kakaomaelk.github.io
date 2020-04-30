# kakaomaelk
Code and ideas for kakaomaelk.dk

First one: December 7, 2019 on a Packet Bell laptop running Fedora 31 server with nginx directon the raw internet with an IP from K-Net from P. O. Pedersen Kollegiet Static ip range.

The file called

    kakaomaelk

is nginx config

# Lets run email as well because it is fun to try

See

    2019-12-12_roundcube-fedora31


stuff to remember

    sudo net-snmp-create-v3-user -ro -A userPWD -a SHA -X cryptPWD -x AES ek
    firewall-cmd --add-rich-rule='rule family="ipv4" source address="82.211.192.0/19" service name="snmp" accept' --permanent

https://www.reddit.com/r/sysadmin/comments/2rjhh0/firewalld_question_how_to_add_source_ip_for/

snmp monitor with v3 user and code, limited access only from k-net

Test SNMP
    snmpwalk -v3 -l authNoPriv -u ek -a SHA -A "userPWD" -x AES -X "cryptPWD" 82.211.200.6

https://www.linickx.com/snmpwalk-v3-and-snmpget-v3-examples
