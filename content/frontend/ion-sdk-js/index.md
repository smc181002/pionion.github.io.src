---
title: 1. ion-sdk-js
toc: true
weight: 5
---

## How to use

https://github.com/pion/ion-sdk-js

### 1. Installation
```
npm install ion-sdk-js
```

### 2. Usage
```
import { Client, LocalStream, RemoteStream } from 'ion-sdk-js';
import { IonSFUJSONRPCSignal } from 'ion-sdk-js/lib/signal/json-rpc-impl';
const signal = new IonSFUJSONRPCSignal("wss://ion-sfu:7000");
const client = new Client(signal);
signal.onopen = () => client.join("test session", "test uid")

// Setup handlers
client.ontrack = (track: MediaStreamTrack, stream: RemoteStream) => {
    // mute a remote stream
    stream.mute()
    // unmute a remote stream
    stream.unmute()

    if (track.kind === "video") {
         // prefer a layer
         stream.preferLayer("low" | "medium" | "high")
    }
});

// Get a local stream
const local = await LocalStream.getUserMedia({
    audio: true,
    video: true,
    simulcast: true, // enable simulcast
});

// Publish stream
client.publish(local);

// mute local straem
local.mute()

// unmute local stream
local.unmute()

// create a datachannel
const dc = client.createDataChannel("data")
dc.onopen = () => dc.send("hello world")

// Close client connection
client.close();
```

### 3. example

#### 1. This show how to write a pubsub example
https://github.com/pion/ion-sfu/tree/master/examples/pubsubtest

Tips: you should open more than two this example for testing


#### 2. This show how to use simulcast and datachannel
https://github.com/pion/ion-sfu/tree/master/examples/echotest
