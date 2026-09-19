NMAP
![Screenshot](images/Pasted%20image 20260916115113.png)

Data Section 
![Screenshot](images/Pasted%20image 20260915235704.png)
	1 can be changed to 0 find other users records

.pcap wireshark creds
![Screenshot](images/Pasted%20image 20260916000149.png)
	-Open username and password in downloadable .pcap file

SSH into user
![Screenshot](images/Pasted%20image 20260916000636.png)
	-We can then SSH into this user in order to get access to the system

Linpeas
![Screenshot](images/Pasted%20image 20260916002206.png)
	-Using scp we can do a file transfer to import linpeas on the ssh session. This reveals that we can run python3.8 allowing us to run whatever python code we want.

Python3.8 Root Escelation
![Screenshot](images/Pasted%20image 20260916002749.png)

Complete Root Access
![Screenshot](images/Pasted%20image 20260916002911.png)
