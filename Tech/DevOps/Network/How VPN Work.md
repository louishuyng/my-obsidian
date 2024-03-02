---
tags: Network, Devops
---
Source: https://www.comparitech.com/blog/vpn-privacy/vpn-tunneling/


A VPN, or Virtual Private Network, encrypts all of the data sent to and from your device and routes it through an intermediary server that stands between you and the internet. The encrypted connection between your device and the VPN server is often referred to as a “tunnel”. No third-parties, such as your ISP, government, or local IT administrator, can see the contents of your data or its destination while the VPN is active.

We’ll discuss how VPN tunneling works in this article, including encryption, protocols, and why tunneling is necessary for security and privacy.

## What is VPN tunneling?

When you first connect to a VPN, your device and the VPN server perform a handshake and exchange encryption keys. This ensures that only the VPN server can decrypt data sent from your device and, conversely, only your device can decrypt data sent from the VPN server.

[![VPN Handshake](https://cdn.comparitech.com/wp-content/uploads/2020/12/VPN-Handshake-01-1024x512.webp)](https://cdn.comparitech.com/wp-content/uploads/2020/12/VPN-Handshake-01.webp)

Once the connection is established, your device and the server can securely transmit data back and forth through the “tunnel”. Data is encrypted with the key before it ever leaves your device. When it reaches the VPN server, it is decrypted, then forwarded to the final destination—a website, app, streaming service, etc.[![How vpn works](https://cdn.comparitech.com/wp-content/uploads/2020/12/How-vpn-works-01-1024x512.webp)](https://cdn.comparitech.com/wp-content/uploads/2020/12/How-vpn-works-01.webp)

Data coming from the internet goes through the same process in reverse: data is sent from the app or website to the VPN server. The VPN server encrypts the data and sends it to your device, where it’s decrypted with the key.

The “tunnel” analogy comes from the VPN’s encryption. Data can go back and forth between the tunnel, but there are only two endpoints—your device and the VPN server—where data is encrypted and decrypted.

## What to look for when choosing a VPN

How you plan on using the VPN determines which tunneling features will best serve you. VPN tunneling can be used for a number of purposes:

- **[Unblocking streaming sites from abroad](https://www.comparitech.com/blog/vpn-privacy/best-vpn-streaming/):** The VPN tunnel should have fast speed and a stable connection. No leaks that could give away your real IP address.
- **[Accessing the web from China](https://www.comparitech.com/blog/vpn-privacy/whats-the-best-vpn-for-china-5-that-still-work-in-2016/):** The VPN tunnel needs to be both inconspicuous and secure. Obfuscation is often used to hide VPN tunnels going in and out of China to bypass the Great Firewall. This also applies in other countries where VPNs are blocked like the UAE and Iran.
- **[Securing public wi-fi](https://www.comparitech.com/blog/vpn-privacy/vpn-public-wifi/):** The tunnel should be secure with no leaks. A kill switch can help keep this tunnel secure.
- **[Torrenting](https://www.comparitech.com/blog/vpn-privacy/is-torrenting-safe-illegal-will-you-be-caught/):** Security and speed are both paramount here. The VPN should have a kill switch, no leaks, and preferably split tunneling.
- **[Private web browsing](https://www.comparitech.com/blog/vpn-privacy/stop-isp-tracking-browsing-history/):** Strong encryption in the VPN tunnel, combined with a no-logs policy and your browser’s incognito or private browsing mode, enables you to surf the web privately and anonymously.

## Split tunneling

Split tunneling is a VPN feature that allows you to choose which data goes through the encrypted VPN tunnel and which uses a direct, unencrypted connection.

A few VPN apps offer split tunneling that allows you to choose which apps use the VPN and which do not. Although whitelisting which apps use the VPN is the most common type of split tunneling, it can also be done by device (at a router level), ports used, or type of traffic.

Split tunneling is useful in situations where only certain activities need to be protected by the VPN. [While torrenting](https://www.comparitech.com/blog/vpn-privacy/best-vpns-for-torrenting-and-p2p-filesharing/), for example, you can set your torrenting app to use the VPN while your web browser uses a normal internet connection.

**See also:** [Best VPNs for split tunneling](https://www.comparitech.com/blog/vpn-privacy/best-vpn-split-tunneling/)

## What are VPN tunneling protocols

A VPN tunneling protocol sets the rules for how your device and the VPN server communicate. Not all protocols are equal, and they each have their advantages and disadvantages. You can often choose between protocols in your VPN app settings.

Here are some of the most [common VPN tunneling protocols](https://www.comparitech.com/vpn/protocols/) in use today:

- **OpenVPN:** an open-source protocol that offers strong security and medium speed, and usually requires a third-party app to use. This is the most popular protocol among consumer VPN apps. Uses SSL encryption.
- **Wireguard:** a newer open-source protocol with fast speeds and decent security, though users’ IP addresses are stored on the server by default. Uses ChaCha20 encryption and usually requires a third-party app. You can find out more in our [Best VPNs with Wireguard](https://www.comparitech.com/blog/vpn-privacy/best-vpn-wireguard/) article.
- **IKEv2:** A medium-speed protocol that’s great at quickly reconnecting after losing signal, which makes it ideal for mobile users. Uses IPSec encryption. Support comes built into many newer devices.
- **L2TP:** A medium-speed protocol that comes built into many popular operating systems like Windows, MacOS, iOS, and Android. Uses IPSec encryption.
- **SSTP:** Similar to L2TP but exclusive to Microsoft systems, such as Windows
- **PPTP:** A fast but insecure protocol that shouldn’t be used due to known security vulnerabilities.

Many VPN apps have multiple protocols available to choose from. Some even have their own proprietary protocols, often based on those above. NordVPN’s NordLynx, ExpressVPN’s Lightway, VyprVPN’s Chameleon, and Hotspot Shield’s Hydra Catapult are all examples of proprietary VPN protocols