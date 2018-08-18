# animated-gif-shrinker
A perl tool to convert an animated GIF into a new image suitable for use as a Slack chat emoji

## Dependencies
 * A [modern perl](https://perlbrew.pl/) installation
 * The [ImageMagick](https://www.imagemagick.org/script/index.php) image manipulation tools

## Usage
```
usage: ags [-i <input>] [-o <output>]
    --input        path to .gif for processing
    -i             

    --output       path to out new gif (default: <input>_output.gif)
    -o             

    --use-every    use every nth frame
    -u             

    --width        new width for image (default: input width)
    -w             

    --height       new height for image (default: scale based on --width)
    -h             

    --fps          set the output frame rate
    -f             

    --from         first frame to use

    --to           last frame to use
```