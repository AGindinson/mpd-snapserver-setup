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
Launch `mpd` and `snapserver`:
```
sudo systemctl start mpd
sudo systemctl start snapserver
# Additionally, hook them into the autostart
sudo systemctl enable mpd
sudo systemctl enable snapserver
```
