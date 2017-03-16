# crunch

*Copyright (c) 2017 Chevy Ray Johnston*

This is a command line tool that will pack a bunch of images into a single, larger image. It was designed for [Celeste](http://www.celestegame.com/), but could be very helpful for other games.

It is designed using libraries with permissible licenses, so you are able to use it freely in your commercial and non-commercial projects. Please see each source file for its respective copyright and license.

### Features

- XML or binary format
- Trim excess transparency
- Rotate images to fit
- Premultiply pixel values
- Recursively scans folders
- Remove duplicate images
- Caching to prevent redundant builds
- Multi-image atlas when the sprites don't fit

### What does it do?

Given a folder with several images, like so:

```
images/
    player.png
    tree.png
    enemy.png
```

It will output something like this:

```
bin/
    images.png
    images.xml
    images.hash
```

Where `images.png` is the packed image, `images.xml` is an xml file describing where each sub-image is located, and `images.hash` is used for file caching (if none of the input files have changed since the last pack, the program will terminate).

There is also an option to use a binary format instead of xml.

### Usage

`crunch [INPUT_DIR] [OUTPUT_DIR] [OPTIONS...]`

For example...

`crunch assets/characters bin/atlases -p -t -v -u -r`

### Options

| option        | alias         | description |
| ------------- | ------------- | ------------|
| -b            | --binary      |  saves the atlas data as .bin file
| -bx           | --binaryxml   | saves the atlas data as both .bin and .xml file
| -p            | --premultiply |  premultiplies the pixels of the bitmaps by their alpha channel
| -t            | --trim        |  trims excess transparency off the bitmaps
| -v            | --verbose     |  print to the debug console as the packer works
| -f            | --force       |  ignore caching, forcing the packer to repack
| -u            | --unique      |  removes duplicate bitmaps from the atlas by hash comparison
| -r            | --rotate      |  enabled rotating bitmaps 90 degrees clockwise when packing

### Binary Format

 ```
 [int16] num_textures (below block is repeated this many times)
        [string] name
        [int16] num_images (below block is repeated this many times)
            [string] img_name
            [int16] img_x
            [int16] img_y
            [int16] img_width
            [int16] img_height
            [int16] img_frame_x         (if --trim enabled)
            [int16] img_frame_y         (if --trim enabled)
            [int16] img_frame_width     (if --trim enabled)
            [int16] img_frame_height    (if --trim enabled)
            [byte] img_rotated          (if --rotate enabled)
```
