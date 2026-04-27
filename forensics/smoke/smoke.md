# Smoke

This was an exceedingly simple challenge created for beginners. The player is given an Apache log file, along with a bash history file, an error log, an FTP banner, an Nmap scan result and a list of open SMB shares. 
The Apache log file simulates an attacker exploiting a file upload vulnerability. One of the GET requests contains the base64-encoded flag in a parameter. The player decodes this to get the flag. 
