# ffmpegWebcamStreamer
Live Streaming Encoder for a Webcam Using FFMPEG as seen on XtendedGreg YouTube: https://youtube.com/live/oQlZpXU7r-U

## Installation
This installation instruction is based on Alpine Linux Raspberry Pi ARMv7 version running on a Raspberry Pi with the apk community repository enabled.  This will allow streaming video and audio from a USB input device like a webcam or HDMI capture device which is UVC supported.
- Install the dependancies
```apk add ffmpeg v4l-utils alsa-utils screen unzip nano```
- Download the zip file of the git repository
```
wget -O ffmpegWebcamStreamer.zip https://github.com/XtendedGreg/ffmpegWebcamStreamer/archive/refs/heads/main.zip
```
- Unzip the zip file
```unzip ffmpegWebcamStreamer.zip```
- Remove Unzip Package
```apk del unzip```
- Copy files to their target locations:
```
cp ffmpegWebcamStreamer-main/Code/bin/ffmpeg-run /bin/ffmpeg-run
cp ffmpegWebcamStreamer-main/Code/etc/init.d/ffmpeg /etc/init.d/ffmpeg
mv /etc/motd /etc/motd.bak
cp ffmpegWebcamStreamer-main/Code/etc/motd /etc/motd
if [ ! -d /etc/ffmpeg-run ]; then mkdir /etc/ffmpeg-run; fi
if [ ! -e /etc/ffmpeg-run/config ]; then cp ffmpegWebcamStreamer-main/Code/etc/ffmpeg-run/config /etc/ffmpeg-run/config; fi
```
- Add executable permissions
```
chmod +x /bin/ffmpeg-run
chmod +x /etc/init.d/ffmpeg
```
- Add files to LBU to persist through reboots
```
lbu add /bin/ffmpeg-run
lbu add /etc/init.d/ffmpeg
```
- Add init as startup service
```rc-update add ffmpeg default```
- Commit files to LBU
```lbu commit -d```
- Copy usercfg.txt to SD Card Root (This will overwrite any existing usercfg.txt file)
```
mount /media/mmcblk0p1 -o rw,remount
cp ffmpegWebcamStreamer-main/Code/media/mmcblk0p1/usercfg.txt /media/mmcblk0p1/usercfg.txt
mount /media/mmcblk0p1 -o ro,remount
```
- Modify the config file to suit your needs
```
# Using Nano
nano /etc/ffmpeg-run/config
# To exit, press CTRL+x and press enter

# Using VIM
vi /etc/ffmpeg-run/config
# Press "i" to edit
# To exit, press "esc" and then type ":wq" (text will appear in the bottom left corner) and press enter
```
- Reboot
```reboot```

## Runtime Commands
- INIT: ```/etc/init.d/ffmpeg [start|stop|restart]```
- SCREEN: ```screen -r ffmpeg```
- The screen session will allow monitoring of the ffmpeg process
- Note: To exit the screen session: press "CTRL+a" and then "d" or close the SSH console window

## Removal Commands
Run these commands from an SSH Console or from the terminal
```
/etc/init.d/ffmpeg stop
rc-update del ffmpeg
rm /bin/ffmpeg-run
rm /etc/init.d/ffmpeg
mount /media/mmcblk0p1 -o rw,remount
rm /media/mmcblk0p1/usercfg.txt
mount /media/mmcblk0p1 -o ro,remount
rm /etc/motd
mv /etc/motd.bak /etc/motd
lbu commit -d
```

## Remove the Config File
```
rm -r /etc/ffmpeg-run
```

## Like, Comment, Subscribe and Share
If you like this project, be sure to check out the linked video and drop a like and subscribe to the XtendedGreg YouTube channel for more fun projects and videos like this.  If you know anyone who might find these projects or videos interesting, be sure to share it with them!
