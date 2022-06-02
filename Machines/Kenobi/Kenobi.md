Samba is the standard Windows interoperability suite of programs for Linux and Unix. It allows end users to access and use files, printers and other commonly shared resources on a companies intranet or internet. Its often referred to as a network file system.

Samba is based on the common client/server protocol of Server Message Block (SMB). SMB is developed only for Windows, without Samba, other computer platforms would be isolated from Windows machines, even if they were part of the same network


If smb, or samba, is open on a machine (ports 139 and 445) then we can use nmap to do some enumeration for us by running this command:
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.140.209 10

*** Insert screenshot of example output ***

Basically, this show us that we can access and mount the /var folder. 

We can connect to a share by using smbclient. 'smbclient //<target-ip>/<share-name>'
'smbclient //10.10.140.209/anonymous


You can recursively download the SMB share too. Submit the username and password as nothing.
smbget -R smb://<ip>/anonymous
smbget -R smb://10.10.140.209/anonymous

Since there is also an ssh key generated for that user, we can potentially pull that ssh key into a location that we can read and access, and then use it to ssh as the user (kenobi).


So if you happen to view a share and there are files on it you want to view, you can download it to your machine byusing the smget command in the above example and it will download everything in that share.

shown port 111 running the service rpcbind. This is just a server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve. 


Since the SUID bit is set on the /usr/bin/menu binary file, then if we run the binary then we will run it as root. We can use this to our advantage so we can get root access. 
*** Insert screenshot of suid ***

*** insert screenshot of running 'strings' command*** 
Running 'strings' on the binary to see if there are any human readable string in the binary. THis just helps give of an idea of any commands this binary might be running etc.  Given that it is running commands such as 'curl', 'ifconfig' without specifying a full path such as (e.g. not using /usr/bin/curl or /usr/bin/uname). Basically it will run 'curl' depending on what is already in our current path variable.

So we can sort of create our own 'curl' binary, or any binary that is being called in the suspicous binary, make it executable and put it up higher in our path...this way whenever we run the supicous binary again, when it runs the curl command it will use whatever comes first in your path variable. So lets just create a bash shell and name it curl and then add it to the path. 

'echo /bin/bash > curl'
'chmod 777 curl'


*** Insert screenshot of getting root shell ***
