Hello! 

If you would like to ship the chat app to the cloud using a DigitalOcean droplet please follow the instructions below: 

If you don't have a DigitalOcean account you can get $10 in credit by signing up with this link: do.co/tammyshark 

Create a droplet: 1GB Ubuntu 15.04 x64. Tick User Data and enter the following cloud-config/metadata/user data script:

``` 
#cloud-config
users:
  - name: demo
    groups: sudo
    shell: /bin/bash
    ssh-authorized-keys:
      - ssh-rsa insertyourkeyhere
runcmd:
  - apt-get update && apt-get upgrade -y
  - sed -i -e '/^PermitRootLogin/s/^.*$/PermitRootLogin no/' /etc/ssh/sshd_config
  - sed -i -e '$aAllowUsers chat' /etc/ssh/sshd_config
  - restart ssh
  - ufw allow 22,80,443
  - ufw enable
  - apt-get install fail2ban -y
  - sed -i -e '/^maxretry =/s/^.*$/maxretry = 3/' /etc/fail2ban/jail.conf
  - sed -i -e '$bantime  = 2419200' /etc/fail2ban/jail.conf
  - service fail2ban restart
  
``` 

<3
