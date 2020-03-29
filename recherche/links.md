### Schneier:
* https://www.schneier.com/essays/archives/2020/01/china_isnt_the_only_.html -> Comment on 5G Security, no technical details
* https://www.schneier.com/essays/archives/2019/09/every_part_of_the_su.html -> Same, lots of links...
* https://www.arnnet.com.au/article/671251/5g-security-mess-could-digital-certificates-help/ -> Jover proposes certificates, Schneier says that's not enough and only one out of a lot more problem

### Roger Piqueras Jover: Senior Security Architect and Security Researcher:
* http://rogerpiquerasjover.net/5G_ShmooCon_FINAL.pdf -> very complex slides, his solution: digital certs / PKI... links to "Insecure connection bootstrapping in cellular networks: the root of all evil."
* https://arxiv.org/pdf/1904.08394.pdf -> "The current state of affairs in 5G security and the main remaining security challenges." -> 04.19 "was muss noch getan werden für security" release 15
* https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8641117 -> "Security and Protocol Exploit Analysis of the 5G Specifications" -> Analysiert die Security Spec von March18 in bezug auf bisherige und neue exploits, 02.19
* https://www.linkedin.com/pulse/reflection-history-cellular-security-research-outlook-piqueras-jover/ -> Gedanken von Jover über die Security Specs etc... 06.19

### 5GReasoner: (vorgestellt 11.19, entwickelt in 19)
* **https://relentless-warrior.github.io/wp-content/uploads/2019/10/5GReasoner.pdf -> 5GReasoner Paper**
* https://techcrunch.com/2019/11/12/5g-flaws-locations-spoof-alerts/ -> Artikel über 5GReasoner
* Case "against" 5GReasoner by Richard Bennett: https://hightechforum.org/wired-trolls-5g-security/

### Dude of 5GReasoner:
* **https://relentless-warrior.github.io/wp-content/uploads/2019/02/paging-side-channel-ndss19.pdf -> paper on ToRPEDO, IMSI-Crack, PIERCER**
* https://relentless-warrior.github.io/wp-content/uploads/2019/05/wisec19-preprint.pdf -> "Insecure connection bootstrapping in cellular networks: the root of all evil." -> No real vulnerabilities. This is just the basis of why it's even possible to set up fake base stations. In this paper they propose a PKI for the authentication.

### General Flaw Stuff:
* **https://arxiv.org/pdf/1806.10360.pdf -> "a formal Analysis of 5G Authentication". komplizierte technische abhandlung der authentication. 5GReasoner erwähnt das mit "however, focus only on a small part of the protocol, i.e., authentication and key agreement (AKA) protocol of the initial registration procedure. Also, the analyses are performed in isolation without considering their interaction with other procedures."**
* https://www.ericsson.com/en/security/a-guide-to-5g-network-security -> ericsson langer artikel mit versch. 5g themen. eher als zusätzliche informationsquelle zu verwenden wenn überhaupt
* https://threatpost.com/torpedo-privacy-4g-5g/142174/ -> ToRPEDO, IMSI-Crack, PIERCER artikel, mitteltechnisch
* https://positive-tech.com/research/5g-security-issues/ -> Security parts not so relevant, but maybe as additional information on 5g
* https://www.cablelabs.com/insights/a-comparative-introduction-to-4g-and-5g-authentication -> additional info on authentication protocl in comparison to 4g


Articles, Information etc // **Actual Flaws**
