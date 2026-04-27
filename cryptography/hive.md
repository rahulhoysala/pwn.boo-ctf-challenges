# Hive

> Count Hackula has realized that the only thing standing between him and complete computational domination were those who resurrected him - ClownSec. To demonstrate his power, he launched a DDoS attack at their infrastructure. Late that night, over strongly brewed bat wing juice, ClownSec's SOC analysts worked tirelessly to shut down the botnet that was hitting them from all sides. They've compiled a list of all the IP addresses that were part of the botnet, but only you can help uncover the secrets between them and help shut down Hackula's threat before it's too late. 

# Resources Provided:

```
98.111.111.123  
100.48.110.55  
95.119.48.114  
114.121.95.101  
118.51.110.95  
115.51.110.116  
49.110.51.108  
48.110.101.95  
119.52.115.95  
102.48.48.108  
51.100.95.98  
121.95.116.104  
49.115.125.0
```

# Solution:

This challenge was largely inspired by the following technique used in the wild: https://www.sentinelone.com/blog/hive-ransomware-deploys-novel-ipfuscation-technique/

An IPv4 address consists of four 8-bit numbers (octets) separated by dots, such as `192.168.1.1`. In memory, this is just a sequence of 4 bytes. By taking a string of text, converting each character to its ASCII decimal value, and grouping them in sets of four, you can disguise text as a list of IP addresses.

Here is a PoC script:

```python
def ip_to_bytes(ip):
    return [int(x) for x in ip.split('.')]

def bytes_to_string(byte_list):
    return ''.join(chr(b) for b in byte_list).rstrip()

def convert_ips_to_string(ips):
    byte_list = []

    for ip in ips:
        byte_list.extend(ip_to_bytes(ip))
    
    return bytes_to_string(byte_list)

def main():
    encoded_ips = ['98.111.111.123', '100.48.110.55', '95.119.48.114', '114.121.95.101', '118.51.110.95', '115.51.110.116', '49.110.51.108', '48.110.101.95', '119.52.115.95', '102.48.48.108', '51.100.95.98', '121.95.116.104', '49.115.125.0']
    
    reconstructed_string = convert_ips_to_string(encoded_ips)

    print("\n[+] Flag:", reconstructed_string)

if __name__ == "__main__":
    main()
```


