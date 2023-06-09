#!/bin/sh

# Functions

edit () {
		ffmpeg -loglevel 24 -i $videofile $1 $output 
}

selector () {
		echo $@ | awk '{print $1}'
}

timestamp_selector () {
		timestamp[i]=$(mpv $videofile |& tee -a /dev/stderr | grep -o ..:..:.. | tail -n 2 | head -n 1) 
}

# Error message constants

ERRORSTATEMENT="INCORRECT USAGE

Usage:" 

ENDERRORSTATEMENT="

Refer to manual for more information"

# Main script

# Output is surpessed when input is incorrect it would give an unintended error
# This way the error message will be only the ones provided above
while getopts "tjcgsxemf" input 2>/dev/null
do
	case $input in

		# Trim video option 
		"t") videofile=$(selector $2)
			 output=$(selector $3)

			 if [[ -z $videofile || -z $output ]]; then 
			 	echo "$ERRORSTATEMENT ffedit -t [VIDEO FILE] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 for (( i=1; i<=2; i++ )); do
			 	timestamp_selector
			 done

			 echo start timestamp: ${timestamp[1]}
			 echo end timestamp: ${timestamp[2]}
			 
             # Encoded (Slower, lower quality, edits are sharp and precise)
             edit "-ss ${timestamp[1]} -to ${timestamp[2]} -c:v libx264";;
             # Copied (Faster, retains original quality, can be glitches in the start and end of the edits)
#             edit "-ss ${timestamp[1]} -to ${timestamp[2]} -c copy";;

		# Concatenate
		"j") mylist=$(mktemp /tmp/mylist-XXXXXXXXXXXXX.txt) 
	   		 concatN=$(selector $2)
			 concatN=$(expr $concatN + 3) 
		 # This is done because in the for loop that comes after we start at i=3,
	   	 # this is because we awk out the 3rd colum first. 
		 # So to compenstate we increase ConcatN by the same distance 3

			 for (( i=3; i<$concatN; i++ )); do
			 	video=$(echo $@ | awk -v i=$i '{print $i}')
				video=$(readlink -f $video)
				echo file \'$video\' >> $mylist
			 done

			 output=$(echo $@ | awk -v i=$concatN '{print $i}')
	
			 if [[ -z $concatN || -z $video || -z $output ]]; then 
				echo "$ERRORSTATEMENT ffedit -j [NUM OF VIDEOS] [VIDEO1] [VIDEO2] ... [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

	         # Encoded (Slower, lower quality, edits are sharp and precise)
            ffmpeg -loglevel 24 -safe 0 -f concat -i $mylist -c:v libx264 $output;;
             # Copied (Faster, retains original quality, can be glitches in the start and end of the edits)
#			 ffmpeg -loglevel 24 -safe 0 -f concat -i $mylist -c copy $output;;

	
		"c") videofile=$(selector $2)
			 output=$(selector $3)
			 finaloutput=$output
 
			 if [[ -z $videofile || -z $output ]]; then 
			 	echo "$ERRORSTATEMENT ffedit -c [VIDEO FILE] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 for (( i=1; i<=2; i++ )); do
			 	timestamp_selector
			 done

			 echo start timestamp: ${timestamp[1]}
			 echo end timestamp: ${timestamp[2]}
			 
			 initaltimestamp=00:00:00
			 
			 clip=$(echo ${output%.*}) 

			 output=$(mktemp $clip.XXXXXXXXXXXXXX.mp4)
			 output1=$output 
			 echo "Processing first clip (This may take a while)"
             # Encoded (Slower, lower quality, edits are sharp and precise)
#			 edit "-y -ss $initaltimestamp -to ${timestamp[1]} -c:v libx264" # -y option is used to overwrite the temporary files immediately
             # Copied (Faster, retains original quality, can be glitches in the start and end of the edits)
			 edit "-y -ss $initaltimestamp -to ${timestamp[1]} -c copy" # -y option is used to overwrite the temporary files immediately

			 finaltimestamp=$(ffmpeg -i $videofile 2>&1 | grep "Duration"| cut -d ' ' -f 4 | sed s/....$//)

			 output=$(mktemp $clip.XXXXXXXXXXXXXX.mp4)
			 output2=$output
			 echo "Processing second clip (This may take a while)"

             # Encoded (Slower, lower quality, edits are sharp and precise)
#   			 edit "-y -ss ${timestamp[2]} -to $finaltimestamp -c:v libx264"
             # Copied (Faster, retains original quality, can be glitches in the start and end of the edits)
   			 edit "-y -ss ${timestamp[2]} -to $finaltimestamp -c copy"


			 echo "Joining videos together"
			 ffedit -j 2 $output1 $output2 $finaloutput

			 rm -f $output1 $output2;;


		# Convert video to gif
		"g") videofile=$(selector $2) 
		 	 output=$(selector $3)
	
			 if [[ -z $videofile || -z $output ]]; then 
				echo "$ERRORSTATEMENT ffedit -g [VIDEO FILE] [OUTPUT NAME] $ENDERRORSTATEMENT"
				exit
			 fi
 
			 edit "-r 50 -f gif";;
	
		# Scale video
		"s") videofile=$(selector $2)
		 	 length=$(selector $3) 
		 	 width=$(selector $4) 
			 output=$(selector $5)

			 if [[ -z $videofile  || -z $length || -z $width || -z $output ]]; then 
				echo "$ERRORSTATEMENT ffedit -s [VIDEO FILE] [LENGTH] [WIDTH] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 edit "-vf scale=$length:$width";;
		
		# Crop video
		"x") videofile=$(selector $2)
			 width=$(selector $3)
			 height=$(selector $4)
			 xpos=$(selector $5)
			 ypos=$(selector $6)
			 output=$(selector $7)
			 if [[ -z $videofile || -z $width  || -z $height  || -z $xpos  || -z $ypos || -z $output ]]; then 
				echo "$ERRORSTATEMENT ffedit -x [VIDEO FILE] [WIDTH] [HEIGHT] [X OFFSET] [Y OFFSET] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 edit "-vf "crop=$width:$height:$xpos:$ypos"";;
	
		# Extract sound 
		"e") videofile=$(selector $2)
			 output=$(selector $3)

	   		 if [[ -z $videofile || -z $output ]]; then  
				echo "$ERRORSTATEMENT ffedit -e [VIDEO FILE] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 edit "-vn -q:a 0 -map a";;
	
		# Mute entire sound or sections
		"m") videofile=$(selector $2)
			 EntireOrSection=$(selector $3) # Checks to see if the user wants to mute the entire video (0) or specific sections (1)
			 output=$(selector $4)
			 n=$(selector $5)

			 if [[ -z $videofile || -z $EntireOrSection || $EntireOrSection == "0" && -z $output ]]; then 
				echo "$ERRORSTATEMENT ffedit -m [VIDEO FILE] [0] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi

			 if (( $EntireOrSection == "0" )); then
			 	edit "-c copy -an"
			 else

				if [[ -z $n ]]; then
					echo "$ERRORSTATEMENT ffedit -m [VIDEO FILE] [1] [OUTPUT NAME/FORMAT] [NUMBER OF SECTIONS] $ENDERRORSTATEMENT"
					exit
				fi

				for (( i=1; i<=$n*2; i++ )); do
			 		timestamp_selector
				done

				# Converts timestamp into seconds 
				for (( i=1; i<=$n*2; i++ )); do
					Secs[i]=$(echo ${timestamp[i]} | cut -b "7 8")
					Mins[i]=$(echo ${timestamp[i]} | cut -b "4 5")
					Hours[i]=$(echo ${timestamp[i]}  | cut -b "1 2")

					MinsConvertedToSecs[i]=$(expr ${Mins[i]} \* 60) # Converts $Mins variable to secs
					HoursConvertedToMins[i]=$(expr ${Hours[i]} \* 60) # Converts $Hours variable to mins
					HoursConvertedToMinsConvertedToSecs[i]=$(expr ${HoursConvertedToMins[i]} \* 60) # Converts the previous $HoursConvertedToMins variable to secs

					totalsecs[i]=$(expr ${MinConvertedToSecs[i]} + ${Secs[i]} + ${HoursConvertedToMins[i]})
				done
	
				# This adds the functionality to add multiple volume=enable.... statements to allow for multiple sections to be muted
				for (( i=1; i<=$n; i++ )); do
					mute[i]=$(echo volume=enable=\'between\(t,${totalsecs[i*2-1]},${totalsecs[i*2]}\)\':volume=0,)
				done
				mute=$(echo ${mute[@]} | sed 's/.$//;s/ //g') # Remove last letter and remove space 

			 	edit "-vcodec copy -af $mute"
			 fi;;

		# Convert formats
		"f") videofile=$(selector $2)
			 output=$(selector $3)

	   		 if [[ -z $videofile || -z $output ]]; then  
				echo "$ERRORSTATEMENT ffedit -f [VIDEO FILE] [OUTPUT NAME/FORMAT] $ENDERRORSTATEMENT"
				exit
			 fi
			

			 edit;;


		"?") if  [[ $@ != "--uninstall" ]] && [[ $@ != "--update" ]]; then

				echo "Invalid option or missing argument 
'ffedit' for help or 'man ffedit' for more detail"
				exit
			 fi;;
	esac

# Update / Uninstall functions

case $@ in
	"--update") read -p "WARNING, if you modified your file updating it will remove local changes, Type (Y/N) to continue: " confirmation
			   if (( $confirmation == "Y" || $confirmation == "y" )); then
			   		update=$(mktemp /tmp/UpdateChecker-XXXXXXXXXXXXXX.txt)
			   		sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit" -o $update

				   	uptodate=$(diff -qs $update /usr/local/bin/ffedit | awk '{print $(NF)}')

				   if (( $uptodate == "differ" )); then
				   		sudo cp $update /usr/local/bin/ffedit
						echo "Updated script!"
				   else 
				   		echo "Script already up to date!"
						exit
				   fi

				   manupdate=$(mktemp /tmp/ManUpdateChecker-XXXXXXXXXXXXXX.txt)
				   sudo curl -sL "https://raw.githubusercontent.com/Edesem/FFedit/main/ffedit.1" -o $manupdate 

				   manuptodate=$(diff -qs $manupdate /usr/local/bin/ffedit | awk '{print $(NF)}')

				   if (( $manuptodate == "differ" )); then
				   		sudo cp $manupdate /usr/local/man/man1/ffedit.1
				   		sudo mandb -q 
						echo "Updated man-page!"
				   else
				   		echo "Man-page already up to date"
						exit
				   fi
				   echo "Up to date!"
			   fi
			   exit
			   ;;

	"--uninstall") read -p "Are you sure you want to uninstall FFedit? (Y/N)" confirmation
				 if (( $confirmation == "Y" || $confirmation == "y" )); then
				 	sudo rm -i /usr/local/bin/ffedit /usr/local/man/man1/ffedit.1
					sudo mandb
				 fi
				 ;;

esac

	exit
done

echo "Usage: ffedit [OPTIONS] [ARGS] ...
Use 'man ffedit' for a detailed manual
		
All options

-t Trim video
-j Concatenate video 
-c Cut video
-g Convert video to gif format
-s Scale video 
-x Crop 
-e Extract sound
-m Mute entire video or section
-f Convert formats

--update
--uninstall"
