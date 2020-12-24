# mpv-concat

## Motivation

Suppose you have a file called concat.txt with the following content:

	# This is a comment
	file 'video.webm'
	inpoint 00:03:02.57
	outpoint 00:10:37.44
	
	# This is another comment
	file 'video.webm'
	inpoint 00:14:53.89
	outpoint 00:23:29.26
	
	# This is another comment
	file 'video.webm'
	inpoint 00:25:32.67
	outpoint 00:32:21.28

This file specifies video segments, based on their filenames,
start-point and end-points, to be concatenated in a final video
with the following command.

	$ ffmpeg -f concat -i concat.txt -c copy out.webm

mpv-concat help the creation of such a file.  With mpv-concat you can
interactively select segments of videos from your mpv video player.


## Usage

If a file named concat.txt exists in the current working directory, it
is read by this script and video segments are get from it, as specified
by [ffmpeg-formats(1)](https://ffmpeg.org/ffmpeg-formats.html#concat).
After opening a file, you can create more video segments in addition to
the ones read from concat.txt (if it exists).  Then, after writing the
segments to disk, a new "concat.txt" file is created in the directory
the mpv command was ran.  The segment start times are indicated in the
mpv timeline as chapters.

**Set time.**
In the video screen, press `Ctrl + T` to grab the first timestamp and
then press `Ctrl + T` again to get the second timestamp. This process
will generate a time range, which represents a video segment.  Repeat
this process to create more segments.

**Print segments.**
To see all the segments made so far, press `Ctrl + P`.  All segments will
appear in the terminal in order of creation, with their corresponding
timestamps.  Incomplete segments will show up as “Segment N in progress”,
where N is the segment number.

**Reset segment.**
To reset an incomplet segment, press `Ctrl + R`.  If the first part of
a segment was created at the wrong time, this will reset the current
segment.

**Delete segment.**
To delete a whole segment, start the segment deletion mode by pressing
`Ctrl + D`.  When in this mode, it's possible to press `Ctrl + NUM`,
where NUM is any number between 0 inclusive and 9 inclusive.  For each
`Ctrl + NUM` pressed, a number will be concatenated to make the final
number referring to the segment to be removed, then press `Ctrl + D`
again to stop the segment deletion mode and delete the segment
corresponding to the formed number.

Example 1: Deleting segment number 3

	Ctrl + D 	# Start segment deletion mode
	Ctrl + 3	# Concatenate number 3
	Ctrl + D	# Exit segment deletion mode

Example 2: Deleting segment number 76

	Ctrl + D 	# Start segment deletion mode
	Ctrl + 7	# Concatenate number 7
	Ctrl + 6	# Concatenate number 6
	Ctrl + D	# Exit segment deletion mode

**Write concat file.**
To write the `concat.txt` file, to be used by ffmpeg, press `Ctrl + W`.
It's important that there are at least one segment, otherwise a blank
file will be created.


## Notes

* This script prevents the mpv player from closing when the video ends,
  so that the slices don't get lost.  Keep this in mind if there's the
  option `keep-open=no` in the current config file.

* This script will also silence the terminal, so the script messages
  can be seen more clearly.


## Log level

Everytime a timestamp is grabbed, a text will appear on the screen
showing the selected time.  When `Ctrl + P` is pressed, besides showing
the slices in the terminal, it will also show on the screen the total
number of cuts (or slices) that were made.  When pressing `Ctrl + W`, a
message will be shown on the screen and the terminal telling that the
`concat.txt` file was written.

**Note:** Every message that appears on the terminal has the log level of 'info'.


## Installation

Since this script changes the chapter list, rather than moving this
script to the mpv script folder (`$HOME/.config/mpv/scripts`), move
this script to another place and create a shell file calling `mpv(1)`
with the `--script=path/to/mpv-concat.lua` option.  Set this shell
file as executable and run it in place of `mpv(1)`.


## Acknowledgments

* Thanks to [pvpscript](https://github.com/pvpscript) for writing
  [mpv-video-splice](https://github.com/pvpscript/mpv-video-splice),
  on which this script is based.
