% FFEDIT(1)

# NAME
ffedit - editing videos through ffmpeg bash script

# SYNOPSIS
ffedit \[OPTIONS\] \[ARGS\]

# DESCRIPTION
**ffedit** is a script that is meant to make editing with ffmpeg a lot less tedious and more streamlined. This a much faster then utilising a full GUI video editor for simple edits.

**ffedit** works from a simple bash script that is easy to understand for even beginners of both unix and ffedit.

# OPTIONS
**-t [Video file] [Output video name]**
: Trim video using mpv. Find the first timestamp with mpv, press q once done and repeat for the second time stamp

**-j [Total amount of videos] [Video1] [Video2] ... [Output File]**
: Concat/join video together. Warning, there may be errors when concatenating videos with different formats. For most ideal results make sure the files are in the same format before concatenating

**-c [Video file] [Output name + format]**
: Cut out a section of a video. Essentially makes you select two sections and uses the preexisting trim and concat features to cut out that section of the video.
	
**-g [Video file] [Output name]**
: Convert video to gif.
	
**-s [Video file] [Length] [Width] [Output name]**
: Scales video. 

**-x [Video file] [Width] [Height] [X Offset] [Y Offset] [Output name]**
: Crop video. In the future I would like to make a or utilise some simple tool to make cropping painless and a lot easier compared to this method.

**-e [Video file] [Output name + format]**
: Extracts sound.

**-m [Video file] [0 or 1] [Output name] [Amount of sections]**
: Mute entire video or just sections. 0 is for muting the entire video. If you choose 1 instead then you will have to state how many sections you want to mute *(for example 1 section equals 2 timestamps, 2 equals 4...)* it will prompt mpv so that you can find the timestamps you desire

**-f [Video file] [Output name + format]**
: Convert video. Simple input the videofile and the output name and whatever format you want it in.

**\--update**
: Will automatically get the latest version of the script and update if your script is out-dated. **Warning: If you have made local changes then you will want to make sure you save as this will override it**.

**\--uninstall**
: Will uninstall the script.

# BUGS
https://github.com/Edesem/FFedit#bugs

If you have a bug report it at:

https://github.com/Edesem/FFedit/issues

# COPYRIGHT
This project is licensed under the GNU General Public License v3.0
