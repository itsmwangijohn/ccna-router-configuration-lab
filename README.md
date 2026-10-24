Router> enable
Router# configure terminal

! Set device hostname and domain name
Router(config)# hostname R1
R1(config)# ip domain-name ccna-lab.com

! Security & Password Policies
R1(config)# service password-encryption
R1(config)# security passwords min-length 12
R1(config)# login block-for 120 attempts 3 within 60

! Create local admin user for SSH
R1(config)# username SSHadmin secret 55Hadm!n2020

! Generate RSA keys for SSH (1024 bit modulus)
R1(config)# crypto key generate rsa
% How many bits in the modulus [512]: 1024

! Privileged EXEC Password
R1(config)# enable secret $cisco!PRIV*

! Console Line Configuration
R1(config)# line console 0
R1(config-line)# password $cisco!!CON*
R1(config-line)# login
R1(config-line)# exec-timeout 4 0
R1(config-line)# exit

! VTY Line Configuration (SSH Only)
R1(config)# line vty 0 4
R1(config-line)# password $cisco!!VTY*
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# exec-timeout 4 0
R1(config-line)# exit

! MOTD Banner
R1(config)# banner motd #!UNAuthorized Access Prohibited! #

! Enable IPv6 Routing
R1(config)# ipv6 unicast-routing

! Interface Configurations (Example based on typical topology interfaces)
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# description Connection to Switch LAN
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:DB8:ACAD:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface loopback 0
R1(config-if)# description Management Loopback Interface
R1(config-if)# ip address 10.10.1.1 255.255.255.255
R1(config-if)# ipv6 address 2001:DB8:ACAD:FF::1/64
R1(config-if)# no shutdown
R1(config-if)# exit

! Save configuration
R1# copy running-config startup-config
