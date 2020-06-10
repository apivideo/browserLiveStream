# browserLiveStream
Use your webcam, browser and NodeJS to stream live video from a webpage to your users.

Based on the [https://github.com/chenxiaoqino/getusermedia-to-rtmp](https://github.com/chenxiaoqino/getusermedia-to-rtmp) codebase, 
his project allows you to stream directly from the browser to your RTMP endpoint (in this case [api.video](https://api.video) is hardcoded in the form.)

Requiremennts:  The website will only work in CHrome and Firefox (Safari/Webkit does not yet support the mediaRecorder API, so unfortunately, no browsers on iPhones will wokr).

## RTMP video
 The Node backend takes the webcam video, and transcodes it into FLV format - so it can be ingested by any "live straeming" site with an RTMP endpoint, which is configurable in the form on the page.

### api.video

The default RTMP endpojnt is a livestream hosted at [api.video](https://api.video).  If you are looking for a streaming provider, check them out - they ahve a strong live streaming service, and (if you want) will also create a video on demand version for later viewing.

Note: The RTMP endpoint in the code is streaming into my account - and I can see all your videos. Please wear pants. :D

## Camera usage on a webpage

In order to use the webcam in the browser, your site must be served via HTTPS, or you users will have to bypass a security message every time they go to yur site (will work in Chrome, but not in Firefox).  

Alternatively: If you host this site locally on your computer, localhost will allow you to use the camera

To launch your own local version on localhost, simply clone the repo and run "node server.js" on the command line. In your browser, go to  localhost:1437 - and you are up and running!

## Use my AWS Image.

For those on AWS, an AMI with the 5 April, 2020 version is available: ami-02da44a3b3a85d357.  witha  git pull you should be all set.  For HTTPS: you'll beed to place a LaodBalancer in front of the EC2 instance.

