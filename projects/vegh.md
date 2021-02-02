---
title: >-
  Cross-platform peer to peer file sharing over the web using Webrtc and React
  Js.
author: Devesh Pawar
date: '2020-08-26'
hero: images/1_GP2sCzGFu9dqHCV-cPaWXg.jpeg
layout: page
---
<!--StartFragment-->

# **My motivation**

Our goal was to make a streamlined easy-to-use peer to peer file sharing web app. Putting more effort into the user’s experience and the simplicity to get things done. The web-app is not just for a particular group of individuals, it’s for the whole community.

> ###### *We wish to achieve a **ZERO THOUGHT** file transfer mechanism where sharing files between two devices or persons, simply does not have to involve thinking about **HOW, WHERE, WHY, and WHAT**.*

## Why all this when there exist so many file-sharing websites?

Well, I thought about this too but all these websites never truly explained where these files are shared or stored. This might be a privacy threat as many people during the current pandemic situation or generally will be sharing confidential documents or files using these services. Using a secure peer to peer connection and its Data channel huge files can be transferred without storing it on any server making it really robust and truly private as only the connected clients/peers are communicating directly with no middle server for transfers.

The peer to peer connection and the data channels are made possible by WebRTC.*WebRTC is basically a global network way to communicate and transfer data to each other.*Which resembles closely to Bluetooth, NFC, and WIFI data sharing. Although using WebRTC we can achieve cross-platform support as it’s web-based.

Let’s dig into WebRTC more.

# **WebRTC**

> **“ WebRTC is a free, open project**that provides browsers and mobile applications with Real-Time Communications (RTC) capabilities via simple APIs. The WebRTC components have been optimized to best serve this purpose. “ —[webrtc.org](http://webrtc.org/)

Well Hypothetically, a “peer to peer” association considers direct information sent between two devices without a requirement for a server to persevere the information. Sounds ideal for our case? 😕 Unfortunately, this isn’t the means by which WebRTC works!

![Image for post](https://miro.medium.com/max/1920/1*1yFueH39CmrY7u9Dh53ZTw.png)

Data transfer using WebRTC

Although WebRTC makes a peer-to-peer connection, despite everything it does require a server known as the**Signaling server**which is utilized to share data about the devices that are expected to connect this with one another. These subtleties can be shared through any conventional information-sharing techniques. WebSockets is favored here as it lessens the inertness in sharing this additional data in an enormous system for setting up an association.

In simple words, the**signalingserver helps in establishing the connection,**however, when the connection is set up, the server no longer approaches the information shared between the associated devices.

A year ago when I started my first WebRTC project it was a bit hard to locate a decent working model that works under “**Production**” levels. So subsequently looking through the web, I found this Youtube channel[Coding With Chaim](https://www.youtube.com/channel/UC7sCgeZ9xOwCGHIp2mVWlUQ). The developer gave really good examples regarding production-ready Webrtc applications.

# **How WebRTC creates a connection (Technically)**

Well, there’s no easy way to explain this but here’s my take on this, Out of the all the considerable number of devices in the network, there must be in any event one device which starts the connection by producing the signal information to be sent to the signaling server. This peer is known as initiator and in simple-peer (module used in this project)`{ initiator: true }`is passed to the constructor when an initiator peer is made.

![Image for post](https://miro.medium.com/max/1600/1*FwE6u3lqjKzxGHWEkj_eKA.png)

Signaling server in action.

When we get the signal information of a peer, this information ought to be some way or another sent to different hubs through a signaling server. Different hubs get this information and attempt to set up an association with the initiator. During this procedure, these peers likewise produce their signal information and are sent to the initiator. The initiator gets this information and attempts to set up a connection with the remainder of the peers.

And voila! 🥳 The devices are now connected and now have a data channel to share information with no middle servers.

Try not to stress on the off chance that you were unable to understand the above workings of WebRTC and how simple-peer abstracts it. It befuddled the hellfire out of me when I just began fiddling around with WebRTC. The up and coming segments are much simpler and elaborative explanation on this.

# **Sharing a file with WebRTC (using simple-peer)**

Websocket server Js code

React Frontend code

Find the whole code on this**[Repo](https://www.youtube.com/redirect?redir_token=QUFFLUhqa2Rwd3c4MkdqaXdRZU43bFRsNU9BaG1fbHBaUXxBQ3Jtc0ttSjJPM1pqUVphUDk5RXlrZnI0YjcwRkFuTF9ZZ0lBRjBKRlNST0RIY3pnZVl4QnVWSkZnV3dfcHozMXVCNlRzM2FjanZqV0k4ejB2bGQwUTlvUzRIeWNTdzdvZmtjbVgyUUE0WXl3NmRZUmgtdlNXZw%3D%3D&v=ZuXmwVXp-dU&q=https%3A%2F%2Fgithub.com%2Fcoding-with-chaim%2Ffile-transfer-final&event=video_description)**. If you attempt the above code in the browser and select some picture file(preferably below 100kb), you’ll see that it promptly downloads it. This is because the peer is in a similar browser and the sender is prompt.

The size of the information sent and got is the equivalent. This shows we had the option to move the whole record in one go! 🥳

# Why Use Array buffer instead of blobs?

In our past code, on the off chance if we pick an enormous file (above 100kb) the document in all probability wouldn’t get sent, this is a direct result of certain constraints of WebRTC channels.

![Image for post](https://miro.medium.com/max/1920/1*x7XvLxh0cRzVV8WRf-LAPA.png)

Array buffer comic illustration by (mozilla.org)

> Each Array buffer has a limit of 16Kb in one go. Which in short means we have to divide the file in to small array buffers. (chop chop 🔪)

Minuscule files can be sent over WebRTC in one go, however, for bigger documents, it is a smart thought to isolate our documents into smaller array buffer and send each piece likewise. Both ArrayBuffer and Blob objects have a cut capacity which makes this simpler. For this, if you look closely in the code we have used a module called stream saver which then converts the array buffer back to a blob.

## Small Note

Since javascript is single-threaded. Handling a large number of the array buffers can cause our beautiful UI to be unresponsive. To fix this we will be using service workers. A service worker is a script that your browser runs in the background, separate from a web page, opening the door to features that don’t need a web page or user interaction. You can learn more about it[here](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers).

Handling array buffers in service workers.

## Advantages of dividing the files into array buffers

While it might feel that dividing the file is only some additional code to make stuff entangled, we get the following advantages that can help in improving our document sharing application.

![Image for post](https://miro.medium.com/max/500/1*N8odUxMf2ORQn_eqMNfQWw.png)

Cross-platform support (Illustration by mozilla.org)

* **Supported on almost all browsers**.
* **Huge document size support**— As referenced prior, this is the essential explanation of why we are implementing it.
* **A superior method to decipher the measure of information sent**— By sending a record in buffers, we would now be able to show the information, for example, level of the document sent, the pace of record sent, and so on.
* **Identify incomplete document sent**— Situations, where a file couldn’t be sent totally, would now be able to be gotten and taken care of in a different way.

# Conclusion

Since we have a straightforward document sharing application utilizing WebRTC which additionally utilizes ArrayBuffer, we should now begin considering the stuff that would prepare our application for production. These details are much meant to be explored rather than just following a straight tutorial.

What can be possibly added more: -

· A Signaling Server (STUN & TURN servers)

· Making the Multiple peer connections scalable.

· A hybrid way to share when WebRTC doesn’t seem to work.

· Increasing the efficiency and speed of transfers.

I hope I have given enough knowledge to get you guys started with your WebRTC applications.😊

Psst… Guys I have built up a Web application that utilizes WebRTC and WebSockets to share files across devices.

It’s called[Vegh](https://veghfile.github.io/)🌪️ and is open-source❤️. I couldn’t imagine anything better than to see your commitments and criticism for the application. Offer some ❤️ on it’s[GitHub repo](https://github.com/veghfile).

Also, did you know? you can hold claps up to 50 times! 😊

<!--EndFragment-->
