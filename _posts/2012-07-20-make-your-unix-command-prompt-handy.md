---
layout: post
header-img: img/default-blog-pic.jpg
author: Gagan
description: 
post_id: 14724
created: 2012/07/20 16:20:25
created_gmt: 2012/07/20 11:20:25
comment_status: open
---

# Make your Unix Command Prompt Handy! 

Unix is considered as a fast operating system for a developer community because of its command line. With just few keystrokes you can do things very fast in comparison to GUI. I have listed below four commands which I find very useful and interesting. I am using bash shell to demo the commands.

  1. **Creating Command Shortcut** There are very common commands which we use repeatedly for example I use "_**ls -lrt**_" command very often to list files in a directory. You can create shortcut for commands using alias command. [sourcecode lang="text"] alias l="ls -lrt" [/sourcecode] To make this alias permanent you need to add this to .bashrc file so that every time alias is available after login. 
    1. open _**.bashrc**_ file in_** vi **_mode and add this command in end of the file.
    2. execute the _**.bashrc**_ file to make this alias working. (. ~/.bashrc).
Aliases also help us in creating one layer of protection. The "rm" command can lead to trouble.You can avoid trouble by creating alias like as shown below. [sourcecode lang="text"] alias rm ="rm -i" [/sourcecode] Now _**rm**_ command will always be working in interactive mode and it will ask for confirmation before deletion. Tools like git and hadoop comes with a lot of shell commands specific to them which are very difficult to remember. You can create shortcuts for them as shown below very easily. I have shown how you can create shortcuts for switching command in git. [sourcecode lang="text"] alias dev="git checkout branch_dev" alias prod="git checkout master" [/sourcecode]
  2. **Creating Command Shortcuts which Accept Parameters** The alias command in bash and ksh shells do not accept parameters. To showcase it let us create new switch command which helps in switching the branch in git. You can create commands which accept parameter branch name by following below listed steps 
    1. Create one directory where you create all your custom commands. [sourcecode lang="text"] mkdir -p /usr/local/mycommands [/sourcecode]
    2. Add directory to path in .bashrc file. Now this folder will be loaded automatically. [sourcecode lang="text"] export path = $PATH:/usr/local/mycommands [/sourcecode]
    3. Open a new file using vi editor [sourcecode lang="text"] vi switch [/sourcecode]
    4. Write below lines in script file switch. [sourcecode lang="text"] #!/bin/bash #$1 here signifies the passed parameter which is branch name in this case git checkout $1 [/sourcecode]
    5. Save the file and give file execution permissions. [sourcecode lang="text"] chmod +x switch [/sourcecode]
You are done now switch command is ready to use.
  3. **Locating a File in Directory Structure** For a long while I have been looking for alternative to find command because find responds very slowly. locate is very fast alternative for _**find**_ command. The difference between locate and find is mentioned below. **find** : find gives dynamic output, it searches into the whole directory structure specified and gives output. **locate** : locate searches for file in already created database. So the output in case of locate will not be dynamic. In case you have recently created one file then locate will not be able to search that file but find does. You can update the database with command _**updatedb**_ to locate recently made changes.
  4. **Printing Directory Structure** _**tree**_ is very useful command it helps in printing the recursive directory structure with all the files present in it. Result is printed in nice graphical representation. This command is very useful while doing deployment or searching some file. Go to the directory for which you want to see the entire structure. [sourcecode lang="text"] cd directory tree [/sourcecode] If this command is not available to you then download this using below command. [sourcecode lang="text"] sudo apt-get install tree [/sourcecode]

We can do things very fast with the help of Unix command line and even faster if we make Unix command prompt our friend. Hopefully the above mentioned things will help you in increasing efficiency with Unix. I love to explore new commands and will keep you posted.