---
tags: Security, Devops
---
Mutual TLS, or mTLS for short, is a method for [mutual authentication](https://www.cloudflare.com/learning/access-management/what-is-mutual-authentication/). mTLS ensures that the parties at each end of a network connection are who they claim to be by verifying that they both have the correct private [key](https://www.cloudflare.com/learning/ssl/what-is-a-cryptographic-key/). The information within their respective [TLS certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) provides additional verification.

## How does mTLS work?

Normally in TLS, the server has a TLS certificate and a public/private key pair, while the client does not. The typical TLS process works like this:

1. Client connects to server
2. Server presents its TLS certificate
3. Client verifies the server's certificate
4. Client and server exchange information over encrypted TLS connection
![[Pasted image 20230708220923.png]]
---
In mTLS, however, both the client and server have a certificate, and both sides authenticate using their public/private key pair. Compared to regular TLS, there are additional steps in mTLS to verify both parties (additional steps in **bold**):

1. Client connects to server
2. Server presents its TLS certificate
3. Client verifies the server's certificate
4. **Client presents its TLS certificate**
5. **Server verifies the client's certificate**
6. **Server grants access**
7. Client and server exchange information over encrypted TLS connection

![[Pasted image 20230708215956.png]]

## Why use mTLS?

mTLS helps ensure that traffic is secure and trusted in both directions between a client and server. This provides an additional layer of security for users who log in to an organization's network or applications. It also verifies connections with client devices that do not follow a login process, such as Internet of Things ([IoT](https://www.cloudflare.com/learning/ddos/glossary/internet-of-things-iot/)) devices.

mTLS prevents various kinds of attacks, including:
- **On-path attacks:** [On-path attackers](https://www.cloudflare.com/learning/security/threats/on-path-attack/) place themselves between a client and a server and intercept or modify communications between the two. When mTLS is used, on-path attackers cannot authenticate to either the client or the server, making this attack almost impossible to carry out.
- **Spoofing attacks:** Attackers can attempt to "spoof" (imitate) a web server to a user, or vice versa. Spoofing attacks are far more difficult when both sides have to authenticate with TLS certificates.
- **Credential stuffing:** Attackers use leaked sets of credentials from a [data breach](https://www.cloudflare.com/learning/security/what-is-a-data-breach/) to try to log in as a legitimate user. Without a legitimately issued TLS certificate, [credential stuffing](https://www.cloudflare.com/learning/bots/what-is-credential-stuffing/) attacks cannot be successful against organizations that use mTLS.
- **Brute force attacks:** Typically carried out with [bots](https://www.cloudflare.com/learning/bots/what-is-a-bot/), a [brute force attack](https://www.cloudflare.com/learning/bots/brute-force-attack/) is when an attacker uses rapid trial and error to guess a user's password. mTLS ensures that a password is not enough to gain access to an organization's network. ([Rate limiting](https://www.cloudflare.com/learning/bots/what-is-rate-limiting/) is another way to deal with this type of bot attack.)
- **Phishing attacks:** The goal of a [phishing attack](https://www.cloudflare.com/learning/access-management/phishing-attack/) is often to steal user credentials, then use those credentials to compromise a network or an application. Even if a user falls for such an attack, the attacker still needs a TLS certificate and a corresponding private key in order to use those credentials.
- **Malicious API requests:** When used for [API security](https://www.cloudflare.com/learning/security/api/what-is-api-security/), mTLS ensures that API requests come from legitimate, authenticated users only. This stops attackers from sending malicious API requests that aim to exploit a vulnerability or subvert the way the API is supposed to function.

## References
[Source ](https://www.cloudflare.com/learning/access-management/what-is-mutual-tls/)

