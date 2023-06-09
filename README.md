<p align="center">
<img src="./FFedit.png" width="1000px">
</p>

<p align="center">Edit videos from the terminal</p>

# Overview
FFedit is a bash script that is meant to streamline editing videos with the FFmpeg program through simple command line arguments.

## Dependencies
FFmpeg, MPV

## Installation
```
sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit" -o /usr/local/bin/ffedit
sudo chmod +x /usr/local/bin/ffedit
sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit.1" -o /usr/local/man/man1/ffedit.1
sudo mandb -q
```

## Update
```
sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit" -o /usr/local/bin/ffedit
sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit.1" -o /usr/local/man/man1/ffedit.1
sudo mandb -q
```

## Uninstall
```
sudo rm -i /usr/local/bin/ffedit /usr/local/man/man1/ffedit.1
```

## Features
Trim videos

Concatenate videos

Cut out a section of a video (remove a section out)

Convert videos to gif's

Scale videos

Crop videos

Extract sound from videos

Mute entire video or just sections

Convert video/audio formats

## Future Features
Make cropping videos less tedious using some sort of program to select what you want to crop (Ideally should be very minimalist)

When concatenating videos give the option to convert everything into one format and then concatenate

Layer sound file ontop of video file 

Layer pictures/videos ontop of video file

Rotate video

Change audio volume 

Allow scaling regardless of aspect ratio

## Bug Reporting
If you notice any bugs simply just post an issue and I'll get around to resolving the issue

## Bugs
**You cannot concatenate videos of different formats/sources** (or atleast its unlikely to successful work).
This is not an error perse but instead just how FFmpeg works, this could be fixed through automatically converting the videos to one format or to let the user convert the videos before hand

## Contributing
Make a pull request or an issue and I'll happily tend to it.

## License
FFedit is under GPL v3.0
