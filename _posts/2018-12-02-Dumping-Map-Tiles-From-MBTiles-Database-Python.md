---
layout: post
title: Dumping Map Tiles from a MBTiles Database with Python
category: Development
tags: [python, programming, mapping, mbtiles]
---

MBTiles is a database format, developed by <a href="https://www.mapbox.com/" target="_blank">Mapbox</a>, for storing tiled data. It’s a relatively simple database format that allows for a convenient, portable way to store map tile data.

Here recently, I’ve been developing code that works with tiled map data, including data contained within an MBTiles database. As part of this, I’ve needed an easy way to dump the map tiles from an MBTiles database to my local disk. It turns out that we can do this quite easily with a little bit of Python, so let’s dig in!

# The MBTiles File Format

The format of an MBTiles database is really pretty simple. In fact, it’s nothing but a SQLite3 database that is formatted in a particular way. This database uses the UTF-8 encoding and contains a few tables for storing the data. For our purposes, however, we only care about the *Tiles* table.

## The Tiles Database Table

The tiles database table must be present in an MBTiles database and must contain the following columns:

* **zoom_level** : *integer*
* **tile_column** : *integer*
* **tile_row** : *integer*
* **tile_data** : *blob*

As you can see, the format of the tile data is actually quite simple!

### One Gotcha

There is one gotcha here that we need to watch out for.

We are commonly familiar with a tile being identified by it’s (Z, X, Y) coordinates, where Z is the zoom level, X is the column and Y is the row. So, a tile would be accessed via a URL: Z/X/Y.png. This is not how the data is formatted in the MBTiles database, however.

MBTiles encodes the zoom_level, tile_column and tile_row according to the <a href="https://wiki.osgeo.org/wiki/Tile_Map_Service_Specification" target="_blank">Tile Map Service Specification</a>. This way of encoding the data is the same in every way, **except with regards to the Y-coordinate**. In the TMS way of doing things, the Y-coordinate is reversed from the “XYZ” coordinate system mentioned above. This is done via the following formula:

$$ y = 2z – y – 1 $$

This just means that we need to remember to convert the Y-coordinate in our program.

# Writing Some Code

Alright, with some of the theory out of the way, let’s actually jump in and start writing some code. We will start by writing the boring boiler-plate code that we don’t care about so much.

## Boring Boiler Plate

```python
import argparse
import logging
import os
import sqlite3

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

# Handle command line args
parser = argparse.ArgumentParser(description="A simple utility to extract files from MBTiles")
parser.add_argument("--input", dest="mbtile_path", help="Path to the mbtile file")
parser.add_argument("--output", dest="output_path", help="Directory to dump tiles to")
args = parser.parse_args()

if not args.mbtile_path or not args.output_path:
    logger.error("You must supply an input and output!")
elif not os.path.isfile(args.mbtile_path):
    logger.error("The input file " + args.mbtile_path + " does not exist.")
elif not os.path.exists(args.output_path):
    logger.error("The output path " + args.output_path + " does not exist.")
else:
    logger.info("Dumping tiles for " + args.mbtile_path + " to " + args.output_path)
    dump_tiles(args.mbtile_path, args.output_path)
```
We’ll walk through this real quick. I’m not going to spend too much time here, since this is just the simple boiler-plate code to get the app started.

Lines 1-4 are our imports. We need argparse to parse the command line arguments, logging to output to the console, sqlite3 for reading the MBTiles database (remember, it’s nothing but a SQLite database) and os for working with the files on disk.

On lines 6-7 we set up our logger and then the rest of the code is simply setting up the parsing for the command line arguments. As you can see on line 24, the actual task of dumping the data will be done by the **dump_tiles(PATH_TO_MBTILES, PATH_FOR_OUTPUT)** function. Let’s go ahead and take a look at that function.

## Implementing the Functionality 

```python
def dump_tiles(mbtilePath, output_path):
    conn = sqlite3.connect(mbtilePath)
    for row in conn.execute('SELECT * FROM tiles'):
        zoom_level = row[0]
        tile_col = row[1]
        tile_row = row[2]
        tile_data = row[3]
        write_tile(output_path, zoom_level, tile_col, tile_row, tile_data)
    conn.close()
```

As you can see, our **dump_tiles** method is pretty straight-forward. On line 1, we open the MBTiles database in a SQLite connection. We then enter a for loop that loops over every entry in the tiles table and store the tile information in some variables (lines 4-8). On line 10, we call another method, **write_tile**, with all of this tile data passed as parameters. We then close the connection to the SQLite database.

So, with these few lines of code, we’ve managed to fetch all of the tile data from the MBTiles database. As you could probably guess, the tiles are actually written to disk in the **write_tile** method.

## Writing the Tiles to Disk

```python
def write_tile(output_dir, zoom_level, column, row, data):
    row = correct_y_value(row, zoom_level)
    path = os.path.join(output_dir, str(zoom_level), str(column))
    if not os.path.exists(path):
        os.makedirs(path)
    f = open(os.path.join(path, str(row) + ".jpg"), 'w+b')
    binary_fmt = bytearray(data)
    f.write(binary_fmt)
    f.close()
```

At this point, we are just about done. We’ve gotten the tile data from the database, now it’s time to write it out to disk. Let’s walk through this code to see what’s happening.

On line 2 you will notice that we are reassigning the row value to be the output from the **correct_y_value(row, zoom_level)** method. This is to correct for the differences in the y-values that we mentioned above. We will take a look at that method in a moment.

In lines 3-5, we are creating the folder structure for the tile. The structure is **output_directory/zoom_level/column**. Next, on line 7, we create a file handle to the file row.jpg. We set this handle to write binary. We then convert the raw tile data into a bytearray and write that binary information to the file handle we just created. We finish up by closing the file handle.

## Fixing the Y-Value

This leaves us with just one last thing to look at, the **correct_y_value(row, zoom)** method.

```python
def correct_y_value(y, zoom):
    y_max = 1 << zoom
    return y_max - y - 1
```
All this method is doing is flipping the Y-value, as described in the section above.

# That's It!

That’s all there is to it! With just around 50 lines of python we’ve managed to create a command-line application that allows us to dump tile data contained in an MBTiles database to disk. It’s worth mentioning that there are a few things we could do to make this utility more robust. For one, tile data could be either JPG or PNG. In our example, we are just assuming it’s stored as JPG data. It would be useful if our utility auto-detected which format to use, but I’ll save that as a future exercise.