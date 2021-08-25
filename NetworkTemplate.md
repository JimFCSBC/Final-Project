# Network Forensic Analysis Report

_TODO_ Complete this report as you complete the Network Activity on Day 3 of class.

## Time Thieves 
You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site?	**Frank-n-Ted-DC.frank-n-ted.com**

   Using Wireshark with the filtering option **ip.addr==10.6.12.0/24** then Edit ==> Preferences ==> Name Resolution and selecting **Resolve network (IP) addresses** gets the following display:

   

   **![image-20210822160339172](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210822160339172.png)**

2. What is the IP address of the Domain Controller (DC) of the AD network? 

   From the same Wireshark filtered display in the screen shot above, from the middle Packet Details section, the Internet Protocol line indicates the DC ip address of **10.6.12.12**

3. What is the name of the malware downloaded to the 10.6.12.203 machine? 

   The file name is **june11.dll** found by using Wireshark filter **ip.addr==10.6.12.203&&http.request.method==GET** , which filters down to 2 packets(from 92K):

   

   

   ![image-20210822183313731](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210822183313731.png)

   Once you have found the file, export it to your Kali machine's desktop. 

   Downloaded the file as follows: 

   **File ==> Export Objects ==> HTTP** which produced many objects, then filtered using **Text Filter: dll**. That file was then saved to the root/Desktop.

   

   ![image-20210822184256558](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210822184256558.png)

   

4. Uploaded the file to [VirusTotal.com](https://www.virustotal.com/gui/). 

5. What kind of malware is this classified as? **A Trojan**

   ![image-20210822184433972](C:\Users\Jim Filsinger\AppData\Roaming\Typora\typora-user-images\image-20210822184433972.png)

   

---

## Vulnerable Windows Machines

1. Find the following information about the infected Windows machine
    - Host name **ROTTERDAM-PC**
    
    - IP address **172.16.4.205**
    
    - MAC address **00:59:07:b0:63:a4**
    
      To obtain the information above, I used Wireshark filter for the DC ip address, **ip.src==172.16.4.4 and kerberos.CNameString**       ![img](https://lh5.googleusercontent.com/eVF6MCKEihnb3gYboQdLOPkS7THOvodBnWtx8BhPDglRtcilOhJ1u0t7TE9r2wHYBPf0PZGWvcawLSfN_s__ieLSfC8BHJ_P8DsClU4eio8HX6_RTmARWA9HkNeD_twvSYxwWL6x=s0)
    
      
    
2. What is the username of the Windows user whose computer is infected? 

    **matthijs.devries**

    ![img](https://lh3.googleusercontent.com/GOl_WICQuCj3uEcTT6M_8bbMGqecnj9LWyeX4_XDmBsukCG0vd5s3WBq8dTo2eCRayCIkJPv59_IozmqorcW44s8xFRJZNIlYw3-mqdjGioqoRw2VQ8hoAdbctFld1a6rJHInxvx=s0)

    

3. What are the IP addresses used in the actual infected traffic? 

    Using conversations statistics and filtering by the highest amount packets between IPs, **166.62.11.64, 172.16.4.205, 185.243.115.84, are the infected traffic**![img](https://lh6.googleusercontent.com/aUuPpgwojs6ZQoTd_BRM8010Z5jYl5S0CRWcSSd3uZkls-ghhpnlDKygCNEI5LtZZrrce7ekkKqEqTwVIvxzwnAKJBkHMUMbfyk11baMOCPRwM4mlHBa5XpQH_750euWy0fBuNz2=s0)

4. As a bonus, retrieve the desktop background of the Windows host.![img](https://lh4.googleusercontent.com/wXtPWR4KjXi__xX9cfOnR2hNSuI-h9kIup7KvtwArliY18YeYseTsnOYWenGvody9qaBoROVNT7y4J3Yl8EJAYYtY-R6_8Kry3kwdZmY4KtGVrG9iLqz2G1zWZLdaOR1siR03WFL=s0)



---

## Illegal Downloads

1. Find the following information about the machine with IP address `10.0.0.201`:
    - MAC address: **00:16:17:18:66:c8**
2. Windows username: **elmer.blanco**
    - Host Name (OS version): **BLANCO-DESKTOP**![img](https://lh5.googleusercontent.com/MQ-hmL1U7_MUuG7Zjp-fNYiBJviiB6e-4j2bmSh_H4xvMckMunHL73t2h8QuG_Isom2KMqOccXjvgFJkxcZ3ICGyFXBC6Ny4kcCY9hyfOavXdLe1iZRWE_3jvtJ6bOB0GfIN5M6Q=s0)
3. Which torrent file did the user download? **Betty_Boop_Rythm_on_the_Reservation.avi.torrent**

![img](https://lh5.googleusercontent.com/09I6EulJ5N7t7P4oUMXEQDGPnjs6Hf6eSGVBGepLIYXg3M5er0tHqu6vKppiCPFYNfliNGgOeER3dwg2tu5Qjdf7qEzDWqScRvawuCswih-x9a5z60bh7LpYlJD8BhbHMlM-6kZq=s0)