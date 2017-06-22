# BluetoothRacer
Controlling a Lego Racer using SBrick with WebBluetooth


## What do I need?

- [LEGO RC Tracked Racer – Set 42065](https://shop.lego.com/en-GB/RC-Tracked-Racer-42065)
- [LEGO Power Functions Extension Wire](https://shop.lego.com/en-US/LEGO-Power-Functions-Extension-Wire-8886)
- [SBrick Bluetooth controller by Vengit](https://www.sbrick.com)
- A browser that support WebBluetooth on your operating system
- Optionally: a Gamepad connected to your computer

Try it out at: https://bricks.blue/tooth

Even if you don't have any of the items above, you can still check it out in emulation mode. It then uses the WebAudio API to hear the tracks spin.

Emulation kinda works in the current version of most browsers, but for the best results you kinda have to use Chrome, Opera of Firefox right now.


## How does this work?

The browser can connect to a Bluetooth LE device like the SBrick using the WebBluetooth API. Each Bluetooth device has a number of services and characteristics. Think of them like objects with properties. Once connected to the device, the API then exposes these services and characteristics and you can read from and write to those characteristics. 

The SBrick exposes a number of services like Device Information which has characteristics for Model name, Firmware and Hardware revision. These are of course read-only. 

It also has a Remote Control service which has certain characteristics for operational information, like the battery level and temperature. But it can also send individual commands to the motors or send a group of commands to turn on and off multiple motors at the same. Those commands are then send from your computer wirelessly to the SBrick which uses the values of the characteristics to turn on motors. 

## But, how does the CSS animation work?

It's actually not that difficult. The animation is applied to the drawing of the car in the DOM. It uses a custom property `--direction` to set the direction. The value of the custom property is then assigned to the `content` property of the `#car` element like this: `content: @var(--direction)`. We can then use `requestAnimationFrame` to query the current state of the animation for each frame using `getComputedStyle`. If the state changes, we send a command to the racer. So the animation runs in the DOM and we get it almost for free.

## And how does the actual steering work?

The Tracked Racer does not have a steering wheel, but instead it has two motors that directly drive the two tracks. To go forward or backwards you drive the two motors in the same direction. To steer you need to drive one of the motors forwards and the other one backwards or vice-versa. It gets a bit tricky because the motors are rotated 180° compared to each other, so if you need to go forward, you need to drive the one clockwise and the other one counter-clockwise. That will make sure they actually turn in the same direction. And for steering both motors need to turn clock-wise or counter-clockwise depending on the direction you want to go in. This makes sure the motors actually turn in the opposite general direction. It's a bit count-intuitive.

## Why??

Because it's fun. And I got to play around with all kinds of new specifications like WebBluetooth, Gamepad API, WebAudio API, Service Workers, CSS Grids, CSS Variables, Viewport units, SVG and animating SVG. Yes, this is a Progressive Web App that fully works offline and can be installed on your Android home screen.

## Based on:

- [SBrick.js](https://github.com/360fun/sbrick.js)
- [Gamepad.js](https://github.com/neogeek/gamepad.js)
