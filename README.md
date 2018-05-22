# Linux Server Configuration - Udacity project
---
## Summary
Linux Server configuration to host catalog project as part of the Full Stack nanodegree program.
This tutorial will describe application hosting on AWS lightsail Ubuntu instance.

#### Resources
- [Amazon AWS Lightsail Ubuntu Linux Instance](https://lightsail.aws.amazon.com/ls/docs/getting-started/article/getting-started-with-amazon-lightsail)
-  [Git](https://git-scm.com/)
- [VirtualBox](https://www.virtualbox.org)
- [OS Linux Ubuntu](https://www.ubuntu.com)

#### Installation instructions:

- Amazon AWS 

    - Create Amazon AWS account: [Amazon AWS Lightsail account](https://portal.aws.amazon.com/) with [Ubuntu linux based instance](https://lightsail.aws.amazon.com/ls/docs/en/articles/getting-started-with-amazon-lightsail). 
    - Create new instance
    - Select instance image (Platform: Linux/Unix, Blueprint: OS only, Ubuntu)
    - Name your instance > **Create**
    - Create Static IP for your instance (in Networking Menu)
    - Name your static IP > **Create** (note your static IP, example: 18.184.238.72)
    - Configure port:

        | Application   | Protocol      | Port Range  |
        | ------------- |---------------| ------------|
        | SSH           | TCP           | 22          |
        | HTTP          | TCP           | 80          |
        | CUSTOM        | TCP           | 123         |
        | CUSTOM        | TCP           | 2200        |

     - Find your DNS address via [reverse IP lookup](https://mxtoolbox.com), Example: ec2-18-184-238-72.eu-central-1.compute.amazonaws.com


- Update Amazon AWS 
     - Connect to your AWS instance via browser in AWS instance **CONNECT** menu (Connect using SSH).
     - type and execute in terminal: 
         - Update the update list: `$ sudo apt-get update` _Fetches the list of available updates_
         - Upgrade packages: `$ sudo apt-get upgrade` _Strictly upgrades the current packages_
         - Upgrade distribution `$ sudo apt-get dist-upgrade` _Installs updates (new ones)_
         - Remove no longer required packages: `$ sudo apt-get autoremove`
         - Reboot the instance with: `$ sudo reboot`
         - Reconnect via Connect using SSH from AWS instance menu.
    - Create new user **`grader`**
        - `$ sudo adduser grader` When prompted enter Full name and Unix password.
             ``` 
            ubuntu@ip-172-26-10-33:~$ sudo adduser grader
            Adding user `grader' ...
            Adding new group `grader' (1001) ...
            Adding new user `grader' (1001) with group `grader' ...
            Creating home directory `/home/grader' ...
            Copying files from `/etc/skel' ...
            Enter new UNIX password: 
            Retype new UNIX password: 
            passwd: password updated successfully
            Changing the user information for grader
            Enter the new value, or press ENTER for the default
            Full Name []: Udacity Student
            Room Number []: 
            Work Phone []: 
            Home Phone []: 
            Other []: 
            Is the information correct? [Y/n] y
            ubuntu@ip-172-26-10-33:~$ 
            ```
        - Use the usermod command to add the user **`grader`** to the sudo group. [How to create a sudo user on ubuntu docs](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)
            - `$ sudo usermod -aG sudo grader`
        - Enter **`grader`** account: `$ su grader` and confirm with grader password
        
    - Configure sshd_conf file on your AWS instance:
        - Add the following entry to sshd_config to disable root to login to the server directly:
        `$ vi /etc/ssh/sshd_config` 
        Edit line `PermitRootLogin prohibit-password` to `PermitRootLogin no`
        Edit line `PasswordAuthentication no` to: `PasswordAuthentication yes`
        Restart ssh service `$ sudo service ssh restart`
        You can try to connect to AWS instance via terminal on you linux PC.
        `$ ssh grader@18.184.238.72` 
    - Configure firewall on your AWS instance. [How to set up a Firewall with UFW on ubunut](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-16-04):
        - To set the defaults used by UFW:
        `$ sudo ufw default deny incoming`
        `$ sudo ufw default allow outgoing`
        - Allowing SSH Connections:
        `$ sudo ufw allow ssh`
        `$ sudo ufw allow 2200`
        - Allowing Other Connections:
        `sudo ufw allow 80` _HTTP on port 80_
        `sudo ufw allow 123` _NTP on port 123_
        - Enable Firewall:
        `$ sudo ufw enable` _Enabling UFW_
        `$ sudo ufw status verbose` _Check Status_
    - Set Up SSH Keys: [How To Set Up SSH Keys on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604)
        - Create the RSA Key Pair (usually your computer): 
        `ssh-keygen`
        
        
        
        
