# Command line practice

1. Unzip the "cmd\_line\_practice" file and put it in your Home directory. Open a command line window and navigate to the unzipped file with `cd ~/cmd_line_practice`
	* If at any point you get lost in the subdirectories, you can return to the root and get your bearings with `cd ~/cmd_line_practice`
	
2. From the root of `cmd\_line\_practice`, navigate to the lowest-level directory. Move `1.txt` up to the root of the directory
	* Required commands: `cd, mv`
	* **Hint:** if you have forgotten where you are in the filesystem, typing `pwd` will show you your location. Another helpful command is `ls`, which prints the names of files in your current directory
	
3. Navigate to `level_2a` and remove `extra_file.txt`. Then, copy the other file, `2.txt`, to the root of the directory
	* Required commands: `cd, cp, rm`
	
4. Navigate up one directory and rename `wrong_name.txt` to `3.txt`. Move `3.txt` to the root of the directory. Navigate to the root of the directory
	* Required commands: `cd, mv`
	* **Hint:** to rename a file, you use the same command that moves files between directories
	
5. Using Vim, create a new file titled `4.txt` in the root directory
	* Required commands: `vi` (or `vim`)
	
6. In `4.txt`, press `Enter/Return`. On a second line, type the following (do not include the quotation marks): "::::FINISHED!::::"

7. Save `4.txt` and exit the file

8. There should now be four `txt` files in the root, `cmd\_line\_practice`. Use `ls` to make sure. Print all these files to screen with `cat *.txt`