# Decent Scale Web SDK

_A client-side JavaScript SDK for the [Decent Scale](https://decentespresso.com/decentscale) by [Decent Expresso](https://decentespresso.com/)_

## Table of Contents  
⚙️ Setting up the SDK  
⚖️ Connecting to your Decent Scale  
🎛 Sending Commands  
👂 Listening for Events  

### ⚙️ Setting up the SDK

Include the following script to your webpage (you can also download the glitch-hosted scripts for local use):  

```
<script src="https://decent-scale-web-sdk.glitch.me/script/DecentScale.js"></script>
```

This will expose a `DecentScale` class you can use to create instances:

```
const decentScale = new DecentScale();
```

### ⚖️ Connecting to your Decent Scale  

You can only connect to your device via Web Bluetooth:

```
const decentScale = new DecentScale();

// the .connect method returns a promise if connected
// make sure to call this method in response to a user action, like a button click
decentScale.connect().then(() => {
  console.log("connected!");
});

// you can also listen for the "connected" event:
decentScale.addEventListener("connected", () => {
  console.log("connected!");
});
```


### 🎛 Sending Commands  

After connecting, you can set the weight/timer visibility, change the displayed weight type, tare the scale, set the timer, and power off:

```
// setLED(showWeight = false, showTimer = false, showGrams = true)
decentScale.setLED();
// show weight, and show it in grams:
decentScale.setLED(true);
// show weight, but in ounces
decentScale.setLED(true, false, false);
// show weight (in grams), and timer
decentScale.setLED(true, true);

// tare
decentScale.tare();

// timer
decentScale.startTimer();
decentScale.stopTimer();
decentScale.resetTimer();

// power off
decentScale.powerOff();

```


### 👂 Listening for Events  

Listening for events is pretty staightforward:
```
// connected
decentScale.addEventListener("connected", () => {
  console.log("connected");
});

// disconnected
decentScale.addEventListener("disconnected", () => {
  console.log("disconnected");
});

// firmware version
decentScale.addEventListener("firmwareVersion", event => {
  const { firmwareVersion } = event.message;
  console.log("firmware version", firmwareVersion);
});

// listen for if the device is powered by usb or battery
decentScale.addEventListener("isUSB", event => {
  const { isUSB } = event.message;
  console.log("isUSB", isUSB);
});

// listen for battery life
decentScale.addEventListener("battery", event => {
  const { battery } = event.message;
  console.log("battery", battery)
});

// weight (in grams), isStable and time (if firmware 1.2)
decentScale.addEventListener("weight", event => {
  const { weight, isStable, time } = event.message;
  console.log("weight", weight);
  console.log("isStable", isStable);
  console.log("time", time.string);
});

// taps (button is "left" or "right", and tap is "short" or "long")
decentScale.addEventListener("buttonTap", event => {
  const {button, tap} = event.message;
  console.log(`${button}: ${tap}`)
});
```