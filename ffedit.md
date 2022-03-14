% FFEDIT(1)

# NAME
ffedit - editing videos through ffmpeg bash script

# SYNOPSIS
ffedit \[OPTIONS\] \[ARGS\]

# DESCRIPTION
**ffedit** is a script that is meant to make editing with ffmpeg a lot less tedious and more streamlined. This a much faster then utilising a full GUI video editor for simple edits.

**ffedit** works from a simple bash script that is easy to understand for even beginners of both unix and ffedit.

# OPTIONS
**-t [Video file] [Output video name + format]**
: Trim video using mpv. Find the first timestamp with mpv, press q once done and repeat for the second time stamp

**-c [Total amount of videos] [Video1] [Video2] ... [Output File + format]**
: Concat video. Warning, there may be errors when concatenating videos with different formats. For most ideal results make sure the files are in the same format before concatenating
	
**-g [Video file] [Output name]**
: Convert video to gif. Make sure for the output name to not include .gif as .gif is included by default
	
**-s [Video file] [Width] [Output name]**
: Scale video. It only scales according to the aspect ratio of the video, therefore you scale it through changing the width

**-x [Video file] [Width] [Height] [X Offset] [Y Offset] [Output name]**
: Crop video. In the future I would like to make a or utilise some simple tool to make cropping painless and a lot easier compared to this method.

**-e [Video file] [Output name + format]**
: Extracts sound.

**-m [Video file] [0 or 1] [Output name]**
: Mute entire video or just sections. 0 is for muting the entire video. If you choose 1 instead then it will prompt mpv so that you can find the timestamps you desire 

# BUGS
**You cannot concatenate videos of different formats/sources** (or atleast its unlikely to successful work).
	This is not an error perse but instead just how FFmpeg works, this could be fixed through automatically converting the videos to one format or to let the user convert the videos before hand

# COPYRIGHT
This project is licensed under the GNU General Public License v3.0
