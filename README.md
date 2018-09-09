# Animated GIF Shrinker
A perl tool to help convert an animated GIF into a new image suitable for use as a Slack chat emoji using ImageMagick

## Dependencies
 * A [modern perl](https://perlbrew.pl/) installation
 * The [ImageMagick](https://www.imagemagick.org/script/index.php) image manipulation tools

## Basic Usage
```
usage: ags -i <input> [-o <output>] [-w <width>] [-f <fps>] [--from <from-frame>] [--to <to-frame>] [-c <bounds>]
    --input        path to .gif for processing
    -i             

    --output       path to output new gif (default: <input>_output.gif)
    -o             

    --use-every    use every nth frame (default: 2)
    -u             

    --width        new width for image (default: input width)
    -w             

    --height       new height for image (default: scaled based on --width)
    -h             

    --fps          set the output frame rate (default: 10)
    -f             

    --from         first frame to use

    --to           last frame to use

    --crop          WxH+X+Y from X,Y grabbing WxH
    -c             
```

## Example

Let's say we have this great GIF of [Andre the Giant](https://en.wikipedia.org/wiki/Andr%C3%A9_the_Giant) declining his opponents' offers to suplex him.

![Andre Says No](example/andre.gif?raw=true "andre.gif : Andre Says 'No'")

This weighs in at ~2MB and is 255 pixels wide.

Slack restricts custom image emoji to <= 64KB and <= 128 pixels width.

Let's start with the default behavior of dropping every other frame and we'll resize to the maximum supported width in Slack (128px).

```
$ ags --input andre.gif --width 128

 * Getting stats for andre.gif
 * Extracting frames...
 * Then using every 2nd frame
 * Resizing frames to 128x?...
 * Assembling new animated GIF...
 * Getting stats for andre_output.gif
 * Cleaning up work files...
 * Done!

 Input:
   * andre.gif
   * 2.0MB
   * 255x191
   * 107 frames

 Output:
   * andre_output.gif
   * 539.3KB
   * 128x96
   * 53 frames

 Delta:
   * 1.5MB smaller
   * 72% smaller

```

Almost a 75% reduction, but we're still too big in terms of file size.  Let's try resizing some more.  When used as responses to messages on Slack, emoji appear smaller than the maximum size.


```
$ ags --input andre.gif --width 64

 * Getting stats for andre.gif
 * Extracting frames...
 * Then using every 2nd frame
 * Resizing frames to 64x?...
 * Assembling new animated GIF...
 * Getting stats for andre_output.gif
 * Cleaning up work files...
 * Done!

 Input:
   * andre.gif
   * 2.0MB
   * 255x191
   * 107 frames

 Output:
   * andre_output.gif
   * 185.7KB
   * 64x48
   * 53 frames

 Delta:
   * 1.8MB smaller
   * 90% smaller
```
 
Great!  We made it 90% smaller, but we've still got too large a file for Slack.  In the GIF there's several shots spliced together, so let's focus and get the best Andre reaction (I opened the GIF in [The GIMP](https://www.gimp.org/) to preview the frames.  You can also just experiment based on the reported frame count):

```
$ ags --input andre.gif --width 64 --from 39 --to 67

 * Getting stats for andre.gif
 * Extracting frames...
 * Then grabbing frames 39 to 67 of input's 1 to 107...
 * Then using every 2nd frame
 * Resizing frames to 64x?...
 * Assembling new animated GIF...
 * Getting stats for andre_output.gif
 * Cleaning up work files...
 * Done!

 Input:
   * andre.gif
   * 2.0MB
   * 255x191
   * 107 frames

 Output:
   * andre_output.gif
   * 54.7KB
   * 64x48
   * 14 frames

 Delta:
   * 1.9MB smaller
   * 97% smaller
```

![Andre Says No](example/andre_output_slow.gif?raw=true "andre.gif : Andre Says 'No'")

Excellent!  By cutting out extra frames and focusing on the best action, we're able to squeeze another 7% out and get our file size down to well under the 64KB maximum Slack imposes, but now the clip looks too slow.

One more tweak to the frame rate and we should have this nailed...

```
$ ags --input andre.gif --width 64 --from 39 --to 67 --fps 8

 * Getting stats for andre.gif
 * Extracting frames...
 * Then grabbing frames 39 to 67 of input's 1 to 107...
 * Then using every 2nd frame
 * Resizing frames to 64x?...
 * Assembling new animated GIF...
 * Getting stats for andre_output.gif
 * Cleaning up work files...
 * Done!

 Input:
   * andre.gif
   * 2.0MB
   * 255x191
   * 107 frames

 Output:
   * andre_output.gif
   * 54.7KB
   * 64x48
   * 14 frames

 Delta:
   * 1.9MB smaller
   * 97% smaller
```

![Andre Says No](example/andre_output.gif?raw=true "andre.gif : Andre Says 'No'")

Awesome, this is exactly what we need!

![Andre No Emoji Slack](example/andre_emoji_slack.png?raw=true "andre_emoji.png : Andre No Emoji Slack")