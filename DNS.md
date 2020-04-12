# Something about DNS
## DNS Intro
DNS - Domain Name System

    Domain Name -> IP
    
Including:
* DNS Client(resolver - DNS解析器)
    * A program, as a part of socket.
    * The browser will invoke the resolver and write the response ip to the browser buffer.
    * **The whole process**: Browser -> Socket -> Protocol Stack(协议栈) -> Network Interface(网卡) -> DNS Server
    * DNS Request include:
        * Domain name
        * Class = IN(Internet)
        * DNS Lookup Types [Common Types](https://support.opendns.com/hc/en-us/articles/227986607-Common-DNS-Request-Types)
* DNS Server
    * 13 root server - Many servers but 13 IPs
    * Every DNS server on the internet will save the root server ip in their configuration file
    * Hierarchy Query, one DNS server could save more than one domain
    * ![](https://www.netnod.se/sites/default/files/2018-06/ROOT%20%281%29.png)
    * DNS Query Sample
    * ![](https://gitlearning.files.wordpress.com/2015/01/dns_3.png)
    
DNS Cache:
* DNS server will cache recently query & save it for a period of time.
* DNS server will tell client whether the ip address is came from the DNS server or Cache.
 
Notice: Http is text message, but DNS is binary message. DNS on port 53 using UDP Protocol
## DNS Leaks
![](https://dnsleaktest.com/assets/img/what-is-a-dns-leak.png)


Traffic go through the VPN, but DNS query is forwarding to ISP DNS, which will log your ip and reveal your location and who are you. 
> Your ISP, every wifi network you've connected to, and your mobile network provider have a list of every site you've visited while using them.

ISP will also using a technology "Transparent DNS proxies" to forces you to use their DNS sevice for all DNS lookups.


![](https://dnsleaktest.com/assets/img/transparent-dns-proxy.png)
## Cloudflare 1.1.1.1
* [Introducing DNS Resolver, 1.1.1.1 (not a joke)](https://blog.cloudflare.com/dns-resolver-1-1-1-1/)
* Extreme fast & privacy
* Datacenter all over the global
 
![](https://blog.cloudflare.com/content/images/2018/03/Cloudflare_and_world-population-map.png)
* Transfer enough info to root server & authoritative server instead of th whole domain name
* Support DNS-over-TLS(port 853) & DNS-over-HTTPS
## DNSSEC

    DNSSEC - Domain Name System Security Extensions - Based on Public Key Crypto

[DNSSEC](https://blog.cloudflare.com/dnssec-an-introduction/) - Great article explain DNS and ATTACKS aim to DNS.
