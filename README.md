# iOS GYRO for Cemuhook

First, I should note that this is a *very* quick and dirty solution that I made for my own use. I only created this repository due to the absence of alternatives for the rest of the community. I apologize for the lack of easily configurable files / configuration screens, etc. Anyhow, I believe you'll still find this application very simple to put to work.

This solution enables the use of iPhone's/iPad's and even Android's GYROS in CEMU (with cemuhook) without jailbreak and without installation of any software in the phone. Since HTML5 provides APIs for WebSocket and for gathering GYRO info, it's enough to visit a web page in the phone (hosted in the local network by your computer) to benefit from the phone's GYRO in CEMU.

## Requesites

As a hardware POV, your phone and computer should be connected in the same local network.

Sofware side, besides CEMU with cemuhook, you only need to have [Node.js](https://nodejs.org/) installed (with its bin folder included into the path, so that you can use `npm` and `node` from the Command Prompt).

### Node.js third-party packages

This is a list with the third-party packages that were used in this project. You do not need to worry about that right now, since they are going to be downloaded into the project's folder with npm in the setup instructions.

 - [ws](https://github.com/websockets/ws)
 - [long](https://www.npmjs.com/package/long)
 - [crc](https://www.npmjs.com/package/crc)

## Setup instructions

Clone this repository into anyware in your computer (avoid Programs Files due to UAC reasons). Navigate to the folder that contains `app.js` using the Command Prompt (`cd <address>`).

Download the required node packages with the following commands (ignore the warnings):

```sh
npm install
```

Finally, run the server with:

```sh
npm start
```

The app.js file implements three servers:

 - An UDP server that communicates with cemuhook (this server listens on the port `26760`)
 - An WebSocket server that communicates with the phone (this server listens on the port `1337`)
 - An HTTP serve that serves the web page to be accessed by the phone (this server listens on the port `8080`)

You can change any ports you want by replacing those values (CTRL+F) in the app.js file with the desired ones. If you change the WebSocket server port, you have to replace it in the `static.html` file as well.

Now, open the `cemuhook.ini` file that is inside the CEMU folder, using a text editor. Change the input section and leave it as follows:

```
[Input]
motionSource = DSU1
motionSourceIsBtnSource = false
serverIP = 127.0.0.1
serverPort = 26760
```

Note that you may have to change the port, if you have changed it in the app.js file.

Finally, launch a web browser in your iPhone/iPad and navigate to the `<computer's_ip_address>:8080` (or `:<port>` with whatever port you have configured). If you locked the screen / closed the browser or, for any other reason, disconnected from the server, just refresh the page to reconnect (on Chrome, you can refresh by just pulling the page down until the refresh icon appears at the top of the screen).

I *strongly adivse* you to lock the rotation to portrait mode in the iPhone.

If you are tuning the scale factor in the `static.html`, I'd recommend opening the page with a private tab. Sice private tabs do not cache, every time you reload you are garanteed to view the most current version.

Enjoy.

## Acknowledgements

I'd like to thank [FrogTheFrog](https://github.com/FrogTheFrog/steam-gyro-for-cemuhook), since his Steam GYRO for Cemuhook provided the base code for the UDP server and a good understanding of the communication protocol used by Cemuhook.

