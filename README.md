# MistServer

MistServer is a streaming media server that works well in any streaming environment even on a Raspberry Pi! It bridges the gap between dedicated media servers and web servers, performing the best of both worlds when it comes to media streaming delivery.

The opensource version works best with HLS live streaming (gaming) with programs such as Open Broadcaster Software (OBS) or Xsplit! If you want more options such as VOD, buy a pro-license.

**What makes this docker build better as the official one from DDTECH/MistServer?**   
It's more secure and because on `docker stop` it will gracefully shutdown your processes. It prevents data corruption and all config-files will be saved before shutting down.

Usage 
-----------
```
docker create --name=mistserver \   
--net=host \    
-v <path to config>:/config \   
-v <path to video>:/media \     
r0gger/mistserver   
```   
  
**Parameters**    
* `--net=host` - Recommended! You can change ports within Mistserver Protocols.
* `-v /config` - config and log files  
* `-v /media` - video and audio files  

*Special note* - If you'd like to run Mistserver without requiring `--net=host` (`NOT recommended`) then you will need the following ports in your `docker create` command:

* `-p 4242` - Web UI  
* `-p 1935` - RTMP  
* `-p 554` - RTSP   
* `-p 8080` - HTTP / HLS 
* `-v /etc/localhost:ro` - for timesync (read-only)   

Run
-----------
1. Webui: http://mydomain.tld:4242 and enter a username/password.   
2. Click on "Enable protocols" and change ports if necessary.
3. Enter a "Human readable name" and set a thick to "Force JSON file save".   

Setup live streaming with RTMP
-----------
1. Go to **Streams** and click **New stream**.   
2. Stream name: obs 
3. Source: `push://` or `push://your-ip-address`   
4. Now go to Open Broadcaster Software and enter to **FMS URL:** `rtmp://mydomain.tld:1935/live/` and **Play Path:** `obs`.   

Embed within website
-----------
```
<video id=autoplay width="720" height="360" class="video-js vjs-default-skin vjs-big-play-centered" controls>
  <source src="http://mydomain.tld:8080/hls/stream_name.m3u8" type="application/x-mpegURL">
</video>
<link href="https://vjs.zencdn.net/7.3.0/video-js.css" rel="stylesheet">
<script src="https://vjs.zencdn.net/ie8/ie8-version/videojs-ie8.min.js"></script>
<script>
var player = videojs('autoplay');
player.play();
</script>
```    
Build Pro version
-----------
1. `git clone https://github.com/R0GGER/mistserver.git mistserver-pro`
2. `nano Dockerfile`
3. `ENV MISTSERVER=https://url-to-pro-version`.
4. `docker build -t mistserver-pro .`

Links
-----------
Website: http://mistserver.org   
Github: https://github.com/DDVTECH/mistserver   
Video.js HLS plugin: https://github.com/videojs/videojs-contrib-hls/releases

