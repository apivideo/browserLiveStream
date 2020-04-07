# browserLiveStream
Use webcam, browser and Node to stream live video

Based on the [https://github.com/chenxiaoqino/getusermedia-to-rtmp](https://github.com/chenxiaoqino/getusermedia-to-rtmp) codebase, 
his project allows you to stream directly from the browser to your RTMP endpoint (in this case [api.video](https://api.video))

Requiremennts:  The website will only work in CHrome and Firefox (Safari does not yet support the mediaRecorder).

In order to use the webcam in the browser, your site must be served via HTTPS, or you users will have to bypass a security message every time they go to yur site (will work in Chrome, but not in Firefox).

For those on AWS, an AMI with the 5 April, 2020 version is available: ami-02da44a3b3a85d357.

To launch your own local version on localhost, simply clone the repo and run "node server.js" on the command line. In your browser, go to  localhost:1437 - and you are up and running!

