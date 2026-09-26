# Lab 0: Basic wireshark operation and capture

## 1. Wireshark Installation Guide (macOS)

1. Go to the official [Wireshark download page](https://www.wireshark.org/download.html).

   ![Wireshark download page](/readMeImages/lab0/image1.png)
   *Wireshark official download page*

2. Click on "macOS universal Disk Image" to start the download.

3. Wait for the download to finish, then double-click on the installer on the desktop.

   ![Wireshark installer](/readMeImages/lab0/image2.png)
   *Wireshark .dmg installer*

4. Drag the Wireshark app into the Applications folder.

   ![Drag Wireshark to Applications](/readMeImages/lab0/image3.png)
   *Installing Wireshark into the Applications folder*

5. Double-click on "Install ChmodBPF.pkg" to install ChmodBPF. This allows Wireshark to capture packets on macOS without requiring root privileges.

   ![Install ChmodBPF](/readMeImages/lab0/image4.png)
   *ChmodBPF installer*

6. (Optional) Double-click on "Add Wireshark to the system path.pkg".

   ![Add Wireshark to system path](/readMeImages/lab0/image5.png)
   *Optional: adding Wireshark to the system PATH*

7. Wireshark is now ready to use.

   ![Wireshark ready to use](/readMeImages/lab0/image6.png)
   *Wireshark launched successfully*

---

## 2. Capture packets: acces the Wikipedia Homepage

![Wikipedia homepage capture](/readMeImages/lab0/image7.png)
*Packet capture while accessing https://www.wikipedia.org*

### Answers

**2.1 Which website did you access?**
The website accessed was en.wikipedia.org.

**2.2 What are the IP address and port number of the website server?**
The IP address of the website is the destination address: 103.102.166.224. The port number is the destination port: 443.

**2.3 What are the IP address and source port number of your PC when initially accessing the website?**
My IP address is the source address: 10.38.196.100. My port number is the source port: 55616.

**2.4 What is the process of the TCP three-way handshake?**

1. The client sends a SYN request to ask the server to initiate a connection.
2. The server replies with a SYN-ACK to let the client know that the connection is established.
3. The client then sends an ACK packet, confirming that the connection between it and the server is working.

![SYN packet](/readMeImages/lab0/image8.png)
*SYN packet — packet 276*

![SYN-ACK packet](/readMeImages/lab0/image9.png)
*SYN-ACK packet — packet 281*

![ACK packet](/readMeImages/lab0/image10.png)
*ACK packet — packet 282*

---

## 3. DNS Packet Analysis

![DNS packet capture](/readMeImages/lab0/image11.png)
*DNS query/response capture*

### Answers

**3.1 What are the IP address and port number of the DNS server?**
The IP address of the DNS server is the destination address: 140.112.254.4. The port number is the destination port: 53.

**3.2 What is the domain name in the DNS query?**
The domain name in the DNS query is www.wikipedia.org.

**3.3 Which protocols does this DNS packet use? List the protocols from Layer 2 to Layer 5 in the TCP/IP five-layer model.**

![DNS packet details pane](/readMeImages/lab0/image12.png)
*Protocols from Layer 2 to Layer 5, displayed in order in the Packet Details pane of Wireshark*

| Layer | Protocol |
|---|---|
| Layer 2 | Ethernet II |
| Layer 3 | Internet Protocol Version 4 |
| Layer 4 | User Datagram Protocol |
| Layer 5 | Domain Name System |

---

## 4. Accessing an HTTP Page

Page accessed: http://www.gzxyzn.com/Article/bjrk2/1644.html

![HTTP page capture](/readMeImages/lab0/image13.png)
*Packet capture while accessing the HTTP page*

### Answers

**4.1 Which HTTP page did you access?**
http://www.gzxyzn.com/Article/bjrk2/1644.html

**4.2 What are the IP address and port number of the server hosting the page?**
The IP address of the server is the destination address: 61.183.8.129. The port number is the destination port: 80.

**4.3 What is the HTTP request method?**
The HTTP request method is GET, as seen in the Info column for frame 308.

**4.4 What is the HTTP response status code, and what does it mean?**

![HTTP response status code](/readMeImages/lab0/image14.png)
*HTTP response — 200 OK*

The HTTP response status code is 200 OK. This means that the client's GET request was successfully received and accepted by the server, and the server is transmitting the requested webpage data (HTML) back to the client.