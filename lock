#!/bin/bash

#Constants
DISPLAY_RE="([0-9]+)x([0-9]+)\\+([0-9]+)\\+([0-9]+)" # Regex to find display dimensions
PARAMS="-colorspace sRGB" # ensure that images are created in sRGB colorspace, to avoid greyscale output
DEFAULT_CACHE_FOLDER="$HOME"/.cache/i3lock-multimonitor/img/ # Cache folder

# Use XDG cache folder if that variable is set.
if [ -z "$XDG_CACHE_HOME" ]
then
    CACHE_FOLDER=$DEFAULT_CACHE_FOLDER
else
    CACHE_FOLDER=$XDG_CACHE_HOME
fi

if ! [[ "$CACHE_FOLDER" == "*/" ]]; then
    CACHE_FOLDER="$CACHE_FOLDER/"
fi

# Create the cache folder if it does not exist
if ! [ -e $CACHE_FOLDER ]; then
    mkdir -p $CACHE_FOLDER
fi

#Passed arguments
while getopts ":i:a:d:" opt; do
    case $opt in
        i) arg_image="$OPTARG"
        ;;
        a) lock_args="$OPTARG"
        ;;
        d) lock_cmd=":"
           arg_image="$OPTARG"
        ;;
        \?) echo "Invalid option -$OPTARG" >&2 && exit 1
        ;;
    esac
done

#Image paths
if [ "$arg_image" ]; then
    BKG_IMG="$arg_image"  # Passed image
elif [ -f "/usr/share/i3lock-multimonitor/img/background.png" ]; then
    BKG_IMG="/usr/share/i3lock-multimonitor/img/background.png"  # Default image
else
    BKG_IMG="$(dirname "$BASH_SOURCE")/img/background.png"  # Fallback to current folder
fi

if ! [ -e "$BKG_IMG" ]; then
    echo "No background image! Exiting..."
    exit 2
fi

MD5_BKG_IMG=$(md5sum $BKG_IMG | cut -c 1-10)
MD5_SCREEN_CONFIG=$(xrandr | md5sum - | cut -c 1-32) # Hash of xrandr output
OUTPUT_IMG="$CACHE_FOLDER""$MD5_SCREEN_CONFIG"."$MD5_BKG_IMG".png # Path of final image
OUTPUT_IMG_WIDTH=0 # Decide size to cover all screens
OUTPUT_IMG_HEIGHT=0 # Decide size to cover all screens

#i3lock command
if [ "$lock_cmd" ]; then
        LOCK_BASE_CMD="$lock_cmd"
else
        LOCK_BASE_CMD="i3lock -i $OUTPUT_IMG"
fi

if [ "$lock_args" ]; then
        LOCK_ARGS="$lock_args"  # Passed command
else
        LOCK_ARGS="-t -e"  # Default
fi
LOCK_CMD="$LOCK_BASE_CMD $LOCK_ARGS"

if [ -e $OUTPUT_IMG ]
then
    # Lock screen since image already exists
    $LOCK_CMD
    exit 0
fi

#Execute xrandr to get information about the monitors:
while read LINE
do
  #If we are reading the line that contains the position information:
  if [[ $LINE =~ $DISPLAY_RE ]]; then
    #Extract information and append some parameters to the ones that will be given to ImageMagick:
    SCREEN_WIDTH=${BASH_REMATCH[1]}
    SCREEN_HEIGHT=${BASH_REMATCH[2]}
    SCREEN_X=${BASH_REMATCH[3]}
    SCREEN_Y=${BASH_REMATCH[4]}

    CACHE_IMG="$CACHE_FOLDER""$SCREEN_WIDTH"x"$SCREEN_HEIGHT"."$MD5_BKG_IMG".png
    ## if cache for that screensize doesnt exist
    if ! [ -e $CACHE_IMG ]
    then
	# Create image for that screensize
        eval convert '$BKG_IMG' '-resize' '${SCREEN_WIDTH}X${SCREEN_HEIGHT}^' '-gravity' 'Center' '-crop' '${SCREEN_WIDTH}X${SCREEN_HEIGHT}+0+0' '+repage' '$CACHE_IMG'
    fi

    # Decide size of output image
    if (( $OUTPUT_IMG_WIDTH < $SCREEN_WIDTH+$SCREEN_X )); then OUTPUT_IMG_WIDTH=$(($SCREEN_WIDTH+$SCREEN_X)); fi;
    if (( $OUTPUT_IMG_HEIGHT < $SCREEN_HEIGHT+$SCREEN_Y )); then OUTPUT_IMG_HEIGHT=$(( $SCREEN_HEIGHT+$SCREEN_Y )); fi;

    PARAMS="$PARAMS -type TrueColor $CACHE_IMG -geometry +$SCREEN_X+$SCREEN_Y -composite "
  fi
done <<<"`xrandr`"

#Execute ImageMagick:
eval convert -size ${OUTPUT_IMG_WIDTH}x${OUTPUT_IMG_HEIGHT} 'xc:black' $OUTPUT_IMG
eval convert $OUTPUT_IMG $PARAMS $OUTPUT_IMG

#Lock the screen:
$LOCK_CMD
