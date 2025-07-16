- **Connection**: Client connects to server on TCP/UDP port 3389.
- **Authentication**: If NLA is enabled (default in modern Windows), the client authenticates **before** loading the full GUI [Wikipedia+1BeyondTrust+1](https://en.wikipedia.org/wiki/BlueKeep?utm_source=chatgpt.com)[Wikipedia](https://en.wikipedia.org/wiki/Remote_Desktop_Services?utm_source=chatgpt.com).
- **Encryption**: Uses TLS or RC4 for encrypting keyboard, mouse, and screen data .
- **Virtual Channels**: Multiple secure channels handle screen updates, clipboard, files, audio, etc. 
## Security Considerations
- **Network Level Authentication (NLA)** blocks unauthenticated sessions – increases security [Wikipedia+1LayerStack+1](https://en.wikipedia.org/wiki/Remote_Desktop_Services?utm_source=chatgpt.com).
- **Encryption** guards against eavesdropping, but older RC4-based encryption is weak; modern versions use TLS [Wikipedia](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol?utm_source=chatgpt.com).
- **High-risk access exposure**: Servers with RDP open to internet are prime targets for brute-forcing, worms, and ransomware **(BlueKeep/DejaBlue)** [remotedesktop.rocketsoftware.com+5Wikipedia+5HelpWire+5](https://en.wikipedia.org/wiki/BlueKeep?utm_source=chatgpt.com).
- **Best practice**: Hide RDP behind VPN or RD Gateway. Use MFA and strong passwords. Don’t expose port 3389 publicly [Wikipedia+3Fortinet+3BeyondTrust+3](https://www.fortinet.com/resources/cyberglossary/remote-desktop-protocol?utm_source=chatgpt.com).