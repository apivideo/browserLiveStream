![image](https://i.imgur.com/1cNMpvg.png)

# HTML5 to RTMP streaming gateway proxy

This project intends to allow an endpoint user to submit RTMP live video streaming directly using web browser and `getUserMedia`, without installing additional software. Currently, only Firefox with `MediaRecorder` API is supported.

## Usage

Start the server by `npm install` and `node server.js`, then open firefox to http://127.0.0.1:8888/ . The rtmp stream will be submitted to rtmp://127.0.0.1/live by default.

Please make sure there's an rtmp server up and running; try `nginx-rtmp-module` if you don't have one.

In production, the server should limit what the client can choose to push stream to.

## How does it work

From `getUserMedia`, `MediaRecorder`, via `socket.io` to `nodejs`, then to `ffmpeg` transcoding and publishing to `rtmp`. You can guess what happened in between.


## Limitation and To-Dos

This is still a relatively primitive project, and a lot of work still need to be done.

- Audio support is experimental, YMMV

Audio stream might get corrupted, and we need more test on the set of FFMpeg parameters. Feel free to open an issue to discuss your experience!

- No resolution adjustment on server-side yet

The server should allow resizing the output video. This can be done by adding output resizing to the list of FFMpeg flags.

- Configurable server with SSL, configurable clients

Hack yourself. Pull request welcomed!

- `socket.io` has bad efficiency doing binary websocket

- Rate-limiting

Currently there's no congestion control of any kind, so this works best in LAN environment.

Consider automatically adjust upstream rate via WebSocket `bufferedAmount` attribute. (Note that locally the rate can only be adjusted by video size...)

##  Create openssl
```
openssl genrsa -out abels-key.pem 2048
openssl req -new -sha256 -key  abels-key.pem -out abels-csr.pem
openssl x509 -req -in abels-csr.pem -signkey abels-key.pem -out abels-cert.pem
```
https://www.youtube.com/watch?v=O3iOWRugHbA

and enjoy

## Server

You can set up your own RTMP server easily via [Nginx-RTMP-module](https://github.com/arut/nginx-rtmp-module), or push to adobe media server / livego server.

## FFMPEG options
You may need to tune FFMPEG's options carefully for specific application need. Here are some brief explanation to common parameters, however there are many complex options possible -- please refer to FFMPEG manual. 

---
		var ops=[
			'-i','-', // Read from STDIN -- corresponding to we're passing raw binary video stream from socket.io to FFMPEG via STDIN pipe
			'-re', // Read input at native frame rate. Mainly used to simulate a grab device. (Reset output frame rate back to normal)
			// Note: you can also set frame rate explicitly by -r 24 or -r 30
			'-fflags', '+igndts', // https://ffmpeg.org/ffmpeg-formats.html
			'-vcodec', 'copy',
			'-acodec', 'copy', // Re-use the codec from browser.
			// Note: you can also choose to re-encode the video here, e,g,:
			// '-vcodec', 'libx264',
			// '-acodec, 'libvorbis', 
			'-preset','ultrafast', // Choosing encoding compression profle. Choose 'slow' to make output stream smaller, at the cost of higher CPU utilization.
			// Available options: ultrafast; superfast; veryfast; faster; fast; medium – default preset; slow; slower; veryslow;
			'-crf' ,'22', // Choosing encoding quality (higher bitrate or lower bitrate)
			// You can also use QP value to adjust output stream quality, e.g.: 
			// '-qp', '0',
			// You can also specify output target bitrate directly, e.g.:
			//'-b:v','1500K',
			'-b:a','128K', // Audio bitrate
			
			socket._rtmpDestination   // Send output stream to this RTMP address
		];
---
    
