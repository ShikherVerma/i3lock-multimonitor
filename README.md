# i3lock-multimonitor-no-text-img
Forked from [ShikherVerma](https://github.com/ShikherVerma)'s [i3lock-multimonitor](https://github.com/ShikherVerma/i3lock-multimonitor).
Basically allows a bit more freedom with the background image for the lockscreen.

It uses [ImageMagick](http://www.imagemagick.org/) to manipulate the given background image and [xrandr](http://www.x.org/wiki/Projects/XRandR/) to gather information about the displays.

## Installation
1. Generally it requires only `ImageMagick` and `i3lock`:
```
sudo apt-get install imagemagick i3lock
```
2. Clone the repository to your local machine and `cd` into the created directory:
```
git clone https://github.com/eyal0803/i3lock-multimonitor.git  # Clone the repository
cd i3lock-multimonitor  # cd into the cloned folder
```
3. Copy the `lock` script (and the `background.png` image included, if you're not planning on giving a parameter) to some place on your system (e.g.: the i3 folder, usually: `~/.config/i3/`):
```
mkdir ~/.config/i3/i3lock  # Just make a new folder for the sake of order
cp lock ~/.config/i3/i3lock  # Copy the lock script
cp background.png ~/.config/i3/i3lock  # Optional. You may pass an argument to the lock script instead
```
4. Give the `lock` script execution permissions:
```
chmod +x ~/.config/i3/i3lock/lock
```

## Usage
```
./lock
```
With image argument:
```
./lock -i ~/Pictures/dog_on_a_bicycle.png
```
With arguments for `i3lock`:
```
./lock -a "-t -p default -e --insidecolor=FF0000FF"
```
Keep in mind that it is not the full `i3lock` command, only the arguments for it.
Also, no need to pass it the `-i` argument since you're passing it to my script (and I pass it over to `i3lock`). You may try though, I don't think it'll break anything.

An example with both:
```
./lock -i ~/Pictures/dog_on_a_bicycle.png -a "-t -p default -e --insidecolor=FF0000FF"
```

## Keybinding
You may also create a keybinding (shortcut) to activate the lockscreen. In your `~/.config/i3/config`:
```
bindsym $mod+l exec "$HOME/.config/i3/i3lock/lock"
```
Be sure to restart i3 by pressing (default) `$mod+Shift+r` right after.

# Notes
**Important:** Since i3lock accepts only images of PNG format my script also accepts only PNG.

I included an image so the script won't crash on your first try.
## Additions and Changes
Added option for passing argument for the path to the image and the argument for the `i3lock` command.
Keeping the aspect ratio of the given image.
