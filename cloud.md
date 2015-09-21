Hello! 

If you would like to ship the chat app to the cloud using a DigitalOcean droplet please follow the instructions below: 

If you don't have a DigitalOcean account you can get $10 in credit by signing up with this link: do.co/tammyshark 

Create a droplet, $10 Ubuntu 15.05 

Use the following 
#cloud-config
users:
  - name: chat
    ssh-authorized-keys:
      - ssh-rsa [i]//put your key here![/i]
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash
runcmd:
  - apt-get update && apt-get upgrade -y
  - sed -i -e '/^Port/s/^.*$/Port 4444/' /etc/ssh/sshd_config
  - sed -i -e '/^PermitRootLogin/s/^.*$/PermitRootLogin no/' /etc/ssh/sshd_config
  - sed -i -e '$aAllowUsers chat' /etc/ssh/sshd_config
  - restart ssh
  - ufw allow 22, 80, 443
  - ufw enable
  - apt-get install fail2ban -y
  - sed -i -e '/^maxretry =/s/^.*$/maxretry = 3/' /etc/fail2ban/jail.conf
  - sed -i -e '$bantime  = 2419200' /etc/fail2ban/jail.conf

<3
