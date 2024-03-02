---
tags: Backend, Technique
---

## Demo
1. A will create an offer (sdp) and set it as local description [[#1. A Create an offer]]
2. B will get the offer and set it as remote description  [[#2. B Get The Offer]]
3. B creates an answer sets it as its local description and signal the 
answer (sdp) to A [[#3. B Create An Answer and Signal Answer to A]]
4. A sets the answer as its remote description [[#4. A sets the answer]]
5. Connection established, exchange data channel [[#5. Exchange Data]]

> A and B here are different browser or different network

## 1. A Create an offer
```javascript
const lc = new RTCPeerConnection()
const dc = lc.createDataChannel('channel')

dc.onmessage = e => console.log("Just got a message: " + e.data)
dc.onopen = e => console.log('Connection opened')

lc.onicecandidate = e => console.log("New Ice Candidate! reprinting SPP" + JSON.stringify(lc.localDescription))

lc.createOffer().then(o => lc.setLocalDescription(o)).then(a => console.log("set successfully!"))
```

## 2. B Get The Offer
```javascript
const offer = {} // Offer JSON get from A create Offer
const rc = new RTCPeerConnection()
rc.onicecandidate = e => console.log("New Ice Candidate! reprinting SPP" + JSON.stringify(rc.localDescription))

rc.ondatachannel = e => {
    rc.dc = e.channel;
    rc.dc.onmessage = e => console.log("new message from client: " + e.data)
    rc.dc.onopen = e => console.log("Connection opened!")
}
rc.setRemoteDescription(offer).then(a => console.log("Offer set!"))
```

## 3. B Create An Answer and Signal Answer to A
```javascript
rc.createAnswer().then(a => rc.setLocalDescription(a)).then(a => console.log("Answer created!"))

```

## 4. A sets the answer
```javascript
const answer = {} // Answer JSON get from B create an answer
lc.setRemoteDescription(answer)
```

## 5. Exchange Data
```javascript
dc.send("Hi B") // A send to B

rc.dc.send("Hi") // B send to A
```