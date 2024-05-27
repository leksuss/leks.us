---
title: "Explaining domain, DNS, NS-server, IP relationship"
date: 2023-07-13T13:01:00+03:00
draft: true
---

This is a common problem with newbies, they don't understand relationship between things in title.

Now I try to explain it :) You have domain, for example, [leks.us](https://leks.us) You write this domain in browser. Next he should find server in the internet where site leks.us is located. I will simplify describing of the process, omit caching on all levels. So, browser gets domain, and he need to find server  IP address where site hosts. Mapping "domain - IP-address" is stores at DNS server. There is a lot of DNS servers in the internet, but [only 13 root DNS servers](https://en.wikipedia.org/wiki/Root_name_server). Also there are DNS servers for top-level domains. For example, there is DNS server for all `.us` domains.

Browser ask nearest DNS server to find IP-address for the domain you entered. If there is no mapping, DNS server ask superior DNS server and so on, while it ask root server. If it couldn't find this domain, it ask DNS server for top level `.us` domain.  If nobody couldn't find this domain, browser return an error like `DNS_PROBE_FINISHED_NXDOMAIN` . It means "I can't find server associated with your domain name". But if any of the DNS servers found record with your domain, it return the IP-address of your web-app (site), and browser going there.

Looks simple, yes? Yes, but not. Because we don't cover how to put mapping "domain - IP-address" to the DNS server. 

#### How DNS server gets mapping "domain - IP-address"?

Well, it's domain owner responsibility. While registering, your domain have registar NS-servers by default. Top level DNS server `.us` zone tell root DNS server about your new domain, so root DNS servers now know about address of your domain NS-servers. **What is NS-servers?** It stores information about IP address of your site (named A-records) and some other things. For example, MX-records responsible for resolving your domain mail server. Or CNAME records, aliases for your domain name. That mean that exactly domain NS-servers stores record about IP address of the domain. And root DNS servers stores do main NS-servers and ask it regularly to check if domain IP is changed. Of course root DNS servers caches this mapping, but it also know about NS-servers as a source of ultimate truth.

Actually, you can set another NS-servers instead of registar. Many hosters gives you free NS-servers. And there is not function difference between it, only usability issues. Also some free mail servers asks you to set it's own NS-servers (actually, I don't know why, because changing MX-servers is enough) if you would like use it's services with your domain name.

Furthermore, you can create your own NS-servers! And set it in domain settings. But be careful with it - if your NS-server is down, the site is down too.

Any DNS server at this chain also has it's own cache. And in many cases browser didn't goes up to the root DNS server. There is browser cache, OS cache, your hosting providers DNS cache, and so on.

So when you change IP address at NS-servers of your domain there is some time to pass to all this caches can pick up new value.