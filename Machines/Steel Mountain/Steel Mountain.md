Introduction

Who is the employee of the month? Bill Harper.
Go to IP address of the machine, 10.10.186.99, then click view source to find the name.


Initial Access

What is the other port running a web server on? 8080
* insert nmap results screenshot

What file server is running? Rejetto Http File Server
Go to the webserver page, 10.10.186.99:8080, and view the source and you will find the rejetto file server.



What is the CVE number to exploit this file server? 2014-6287 
Just do a quick google search of rejetto filer server vulnerability


What is the user flag?
04763b6fcf51fcd7c13abc7db4fd365

Found the user flag in the Users/Bill/Desktop/user.txt 






Privelage Escalation 

Switch to Windows/Tasks directory and upload the PowerUp.ps1 file. Insert screenshot 


Take close attention to the CanRestart option that is set to true. What is the name of the service which shows up as an unquoted service path vulnerability? AdvancedSystemCareService9

Note: The CanRestart option being true, allows us to restart a service on the system, the directory to the application is also write-able. This means we can replace the legitimate application with our malicious one, restart the service, which will run our infected program!







Commands used:


To enumerate this machine, we will use a powershell script called PowerUp, that's purpose is to evaluate a Windows machine and determine any abnormalities - "PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations."
upload /home/kali/Downloads/PowerUp.ps1


What is an unquoted service path vulnerability?


<?php system($_REQUEST["cmd"]) ?>