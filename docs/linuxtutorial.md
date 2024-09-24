All credits for this HANDS-ON go to Mag Selwa from KULeuven.  
[Check out the whole repo including slides](https://github.com/hpcleuven/Linux-intro/tree/master).


### HANDS-ON 1

| Task | Hint |
|------|------|
| 1. Open the VM and play Linux (Ubuntu) | <details><summary>Show Hint</summary> Check the Applications menu. Run the command line (Show applications -> Terminal).</details> |
| 2. Identify if you are a normal user or a superuser | <details><summary>Show Hint</summary> Try to switch to root (superuser) and report what happens. `whoami`, `su`</details> |
| 3. Identify your shell | <details><summary>Show Hint</summary> `echo $SHELL`</details> |
| 4. Check the kernel version, system distribution, and glibc version | <details><summary>Show Hint</summary> `uname -r`, `uname -a`, `ls -l /lib/i386-linux-gnu/libc.so*`</details> |

### HANDS-ON 2

| Task | Hint |
|------|------|
| 1. Start Terminal and display the list of your files and directories | <details><summary>Show Hint</summary> `ls`</details> |
| 2. Check more options about the `ls` command and display all files | <details><summary>Show Hint</summary> `ls --help`, `man ls`, `info ls`, `ls -a`</details> |
| 3. Clear the screen | <details><summary>Show Hint</summary> `clear`</details> |
| 4. Compare the information given by different kinds of help about `pwd` command | <details><summary>Show Hint</summary> `whatis pwd`, `help pwd`, `man pwd`, `info pwd`</details> |
| 5. Navigate directories: Downloads, Desktop, and back to home | <details><summary>Show Hint</summary> `cd Downloads`, `ls`, `cd ../Desktop`, `ls`, `cd ../Downloads`, `cd ~`, `cd`</details> |
| 6. Review a few used commands with arrow and compare with output from history | <details><summary>Show Hint</summary> `history`</details> |
| 7. Print the date on the screen and save it to a file | <details><summary>Show Hint</summary> `date`, `date > date.txt`, `date >> date.txt`</details> |
| 8. Exit the shell | <details><summary>Show Hint</summary> `exit`</details> |
| 9. Start terminal again and perform directory operations | <details><summary>Show Hint</summary> `pwd`, `cd Desktop`, `pwd`, `cd ..`, `pwd`, `cd /tmp`, `pwd`, `cd /home/student`, `ls /`</details> |
| 10. Display more information about `date.txt` file | <details><summary>Show Hint</summary> `ls -l d*`, `stat date.txt`</details> |

### HANDS-ON 3

| Task | Hint |
|------|------|
| 1. Create and manipulate directories and files | <details><summary>Show Hint</summary> `mkdir CourseLinux`, `cp -r -v -i /usr/share/icons/ ./CourseLinux`, `mkdir CourseLinux/test`, `mkdir CourseLinux/test1`, `mv CourseLinux/test1 CourseLinux/test2`, `cp -r -v -i /usr/share/icons/* CourseLinux/test`, `mv CourseLinux/test/unity-icon-theme/apps/128/photos.svg CourseLinux/test/fl.svg`, `cp -v -i CourseLinux/test/fl.svg fl2.svg`, `display fl2.svg`, `ln -s CourseLinux/test/fl.svg mylink2file`, `display mylink2file`, `rm -i CourseLinux/test/fl.svg`, `display mylink2file`</details> |
| 2. Clear the screen and perform file operations | <details><summary>Show Hint</summary> `clear`, `cd CourseLinux`, `wget https://raw.githubusercontent.com/hpculeuven/Linux-intro/master/tabel.dat`, `wget https://raw.githubusercontent.com/hpculeuven/Linux-intro/master/matstats.log`, `cat tabel.dat`, `less matstats.log`, `tail -30 matstats.log`, `head matstats.log`, `grep ardaa matstats.log`, `grep ardbb matstats.log`, `ls -al | less`, `ls -al | grep tabel.dat`</details> |
| 3. Create a file called `test4edit` and edit it | <details><summary>Show Hint</summary> `touch test4edit`, `gedit test4edit`, `nano test4edit`, `vi test4edit`</details> |

### HANDS-ON 4

| Task | Hint |
|------|------|
| 1. Show the path of gcc command and perform various operations | <details><summary>Show Hint</summary> `which gcc`, `whereis gcc | grep lib`, `whoami`, `echo "I like Linux" | tr -d 'I' | tr a-z A-Z`, `bc`, `date > date.txt`, `date >> date.txt`, `cp date.txt date1.txt`, `echo "I like Linux" >> date1.txt`, `echo "And I do not" >> date.txt`, `diff date.txt date1.txt`, `du -kah CourseLinux`, `wc date.txt`</details> |
| 2. Clear the screen, archive and unpack directories | <details><summary>Show Hint</summary> `clear`, `cd ~/CourseLinux`, `ls ico*`, `tar -cvf ico.tar icons/`, `tar -czvf ico.tar.gz icons/`, `mkdir newtest`, `cd newtest`, `cp ../ico.tar.gz .`, `tar -xzvf ico.tar.gz`</details> |
| 3. Work with permissions and directory contents | <details><summary>Show Hint</summary> `cd ~/CourseLinux`, `mkdir testfiles`, `cd testfiles`, `touch file1`, `gedit file1`, `cp file1 file2`, `chmod u-w file2`, `ls -la`, `gedit file2`, `cd ..`, `chmod u-w testfiles`, `ls -la`, `cp testfiles/file1 testfiles/file3`, `chmod o-rwx testfiles`, `ls -la`, `ls testfiles`, `chmod u-r testfiles`, `ls -la`, `cd testfiles`, `cd ..`, `chmod u-x testfiles`, `ls -la`, `cd testfiles`</details> |
| 4. Manage processes and run applications | <details><summary>Show Hint</summary> `clear`, `ps -aux`, `gedit`, `ps -u student | grep gedit`, `kill <pid>`, `gedit &`</details> |

### Summary

- All the Hint sections have been updated with `<details>` and `<summary>` tags for collapsible content.
- Users can click on "Show Hint" to reveal the hint and click again to collapse it.

Make sure to rebuild your MkDocs project after updating the Markdown file to see the changes.