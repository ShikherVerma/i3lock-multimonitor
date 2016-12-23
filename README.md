# i3lock-multimonitor
This is a script which uses a background image specified in env variable `$I3LOCK_BACKGROUND`, resizes it to show correctly on multimonitor setup.

The idea for this project was shamelessly copied from [guimeira](https://github.com/guimeira)'s [i3lock-fancy-multimonitor](https://github.com/guimeira/i3lock-fancy-multimonitor).

It uses [ImageMagick](http://www.imagemagick.org/) to resize `$I3LOCK_BACKGROUND` image and adds a lock icon and text.

By using information from [xrandr](http://www.x.org/wiki/Projects/XRandR/) and basic math, this script supports multiple monitor setups, displaying the icon and text centered on all screens.

The original project adds a different lock image than i3lock, by add transparent black circle around it. The text is also an image, making it easier to customize (and to put it at the correct position). Finally, it uses vanilla [i3lock](https://github.com/i3/i3lock). If you want to customize the colors of i3lock, the recommended version of i3lock-color is [this one](https://github.com/Arcaena/i3lock-color), maintained by [Chris Guillott](https://github.com/Arcaena).

## Installation
Make sure you have all the dependencies:
Note - Instructions are for Ubuntu since its the most popular linux distro.
```
sudo apt-get install imagemagick i3lock
```
Copy the `lock` script along with the images to some place on your system (e.g.: the i3 folder) and give it execution permission:
```
git clone https://github.com/shikherverma/i3lock-multimonitor.git
cp -r i3lock-multimonitor ~/.i3
chmod +x ~/.i3/i3lock-multimonitor/lock
```
Add a env variable in your `~/.bashrc` by running :  
Note - you have to replace `<path to some image>` in the command below with your own path
```
echo "export I3LOCK_BACKGROUND=<path to some image>" >> .bashrc
```
Create a key binding on your i3 config file (in this example I'm using $mod+p):
```
echo "bindsym \$mod+p exec /home/$USER/.i3/i3lock-multimonitor/lock" >> ~/.i3/config
```
Now reload the i3 configuration file. By default, the key binding is `$mod+Shift+c`.
