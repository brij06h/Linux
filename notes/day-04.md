# What i learned 
 I learned how to automate folder creation, file creation, and writing content using a bash sccript in linux.

## script :

	#!/bin/bash

	#get script directory
	script_dir="$(cd "$(dirname "$0")" && pwd"

	#crete folder
	mkdir -p "$script_dir/my_folder"

	#create file and write text inside it
	echo "brijesh is here, learning linux and bash" > ""$script_dir/my_folder/message.txt

	echo "Task completed successfully!"

## To run script
	
	nano script.sh	# create file
	chmod +x scrip.sh	# give permission
	./script.sh	# run script
