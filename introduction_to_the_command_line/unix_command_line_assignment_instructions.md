---
output:
  pdf_document: default
  html_document: default
---
\raggedright

Unix command line assignment
============================

To earn a micro-badge for this workshop (or just practice your skills!), 
complete the following steps. **All steps must be completed with a command line interface.**

1. Unzip `command_line_assignment.zip` and put it in your Home directory. Open a
terminal window and navigate to the unzipped directory. Once there, clear your 
shell history with the following:
    + If you are using Bash, enter `history -c`
    + If you are using Zsh, enter `history -p`
    + If you don't know what shell you're using, type `echo $0`. It will print out 
    the current shell
  
2. From the top level of `command_line_assignment`, navigate to the lowest 
subdirectory. Move `1.txt` up to the top of the directory.

3. Navigate to `level_2a` and remove `extra_file.txt`. Then, copy the other file, 
`2.txt`, to the top of the directory.

4. Navigate up one subdirectory from `level_2a` and rename `wrong_name.txt` to 
`3.txt`. Move `3.txt` to the top level of `command_line_assignment`.

5. Navigate to `level_2b`. Make note of the name of the dotfile in this folder.

6. Return to the top level of `command_line_assignment`. Using Vim, create and 
open a new file titled `4.txt`. Enter `Insert` mode and press `Return/Enter`. 
On a second line, type the following (do not include quotations): “::::FINISHED!::::”.

7. Skip two lines in Vim. Write the name of the dotfile in `level_2b`.

8. Skip another two lines. In a few sentences, explain the difference between a 
relative and absolute path. Given an example of each.

9. Save `4.txt` and exit the file.

10. There should now be four `.txt` files in the top level of `command_line_assignment`. 
Use a command to print the directory contents to screen and make sure. Then, print 
the _contents_ of these files to the terminal window.
    + You can use `*.txt` to apply a command to all text files in a folder
    + Remember that there are two different commands for inspecting directory 
    contents and file contents
  
11. Send the output of the file contents to a new file titled `complete.txt`. You 
do so by running the command to print all the file contents with `> complete.txt` 
added at the end (full syntax: `[command] *.txt > complete.txt`).

12. Open `complete.txt` with Vim. If you do not see your answers from above in 
this file, you will need to try this step again (perhaps with a different command).

13. Export your command line history with `history > command_line_history.txt`.

14. If you're applying to earn a micro-badge for the Introduction to Unix Command 
Line, submit `complete.txt` and `command_line_history.txt` to the GradPathways 
portal. The link to do so is available at [https://gradpathways.ucdavis.edu/microbadge-unix-command-line-submission-guide](https://gradpathways.ucdavis.edu/microbadge-unix-command-line-submission-guide).