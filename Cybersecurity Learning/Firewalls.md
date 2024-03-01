
If your engine explodes and catches fire, there's a firewall that shields you from the burning hot flames

Network firewalls work in the same way!

### Traditional Firewalls

Ex:
- We have two hosts, a switch and a router. Because you have control over these devices, its reasonable to consider as a trusted network. 
- But we have zero control over the devices and networks outside of our own. These would be untrusted networks. 
- While most of the world consists of good people, there are plenty of bad guys that want to get a hold of your system.
- With routers generally having minimal security systems, you can quickly be at the mercy of attackers. THIS IS WHERE FIREWALLS COME IN!

Firewalls are designed to shield and protect networks from the untrusted. 
- A firewall will block all of the bad traffic and at the same time allow normal flow for good traffic

How do firewalls achieve this?
- Most FW's block everything by default, no matter if its leaving or entering the network
- The way we allow traffic to pass the FW is by adding firewall rules. 
- Ex: we may add a rule where the source is 'host a', destination is 'any', and the port is 'http' or 'https' with an action of 'allow'. When host A sends traffic, the FW will see this and ensure the criteria of the FW rule is met before allowing the traffic through. If B tries to send traffic, the FW sees there are no matching rules, and blocks the traffic. 

What about traffic entering the network?
- Its a good idea to block traffic from all external sources except special cases (email servers, web servers, etc)
- But even still, they should be tightly controlled by the FW rules
- This presents an issue: lets say we request a web page. The FW checks the web page and allows the traffic out. At some point there will be a response but if the FW doesn't allow inbound traffic, what happens?
- Most FWs are stateful, meaning they monitor active connections. Once outbound traffic has been allowed, the returning traffic is accepted. 

### Next Generation Firewalls (NGFWs)

Enhances traditional FW's with advanced security features

Different NGFWs may have different features but they should generally all include the following:
- Application level inspection - allows the FW to identify and block risky application traffic
- Intrusion prevention systems (IPS) - inspects the contents of the traffic and look for patterns or signatures, looking for malicious or malware related traffic. Can also detect anomalies and unusual traffic.
- Threat intelligence - NGFWs can update themselves from external threat intelligence sources. If a brand new attack has been identified, the vendor can update the FW's threat intelligence to identify this new emerging threat. 

Other features that NGFWs may offer include:
- URL filtering
- Email scanning
- Data loss prevention (DLP)

Firewalls that have features like these three above are known as UTMs, or unified threat management

### Software-based Firewalls

Computers can have software-based firewalls

Ex: the Windows FW is built-in and uses the same firewall methods as a traditional. So why have a network and an endpoint firewall?
- It's like a house. If you locked all of your internal doors, would it make sense to leave the external door open? No!