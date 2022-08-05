# mpd-snapserver-setup
# Setup
Install software:
```
sudo apt-get update
sudo apt-get install -y icecast2 mpd mpc snapserver
```
Configure `mpd`:
- Run`sudo nano /etc/mpd.conf`
- Find the default `audio_output` block & make sure each line of the
  block is commented out by single `#`
- Set `mpd`'s output onto Snapserver's FIFO pipe by inserting the
  following `audio_output` block alongside:
```
audio_output {
    type            "fifo"
    name            "my pipe"
    path            "/tmp/snapfifo"
    format          "48000:16:2"
    mixer_type      "software"
}
```

Also adjust `snapserver` configuration:
- Run `sudo nano /etc/snapserver.conf`
- Find and comment out the line(s) beginning with `stream =`
- Alongside, add the following line:
```
source = pipe:///tmp/snapfifo?name=Radio&sampleformat=48000:16:2&codec=flac
```
- Note that actual values for `sampleformat`, `codec`, etc. may have
  to be adjusted depending on the specific Icecast2 stream parameters.

Launch `mpd` and `snapserver`:
```
sudo systemctl start mpd
sudo systemctl start snapserver
# Additionally, hook them into the autostart
sudo systemctl enable mpd
sudo systemctl enable snapserver
```
# Launching MPD to Snapserver playback
```
mpc add http://<icecast2_stream_address>
mpc volume +50 # By default, it's set to 0; 100 is the maximum value
mpc play
# If needed, toggle off with mpc stop
```