---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 9599
created: 2011/09/11 18:06:20
created_gmt: 2011/09/11 13:06:20
comment_status: open
---

# Identifying GIT Merge Conflicts with a Merge Tool

One of the pain points in GIT **was** resolving merge conflicts. I have highlighted ‘was’ because at the end of this blog you will find your life easy the next time you encounter merge conflicts. Working with GIT in agile development increases the probability of merge conflicts, and the reason behind is straight forward – Parallel Development.  As I mentioned in my earlier [blog][1], there are lot of advantages of using GIT branching model. But one of the pain points I mentioned was resolving merge conflicts. One has to manually open the files and identify conflicts. In this blog, I will share how you can configure and open a 3-way merging tool to identify your merge conflicts. 

Why a 3-way merging tool? You always have two branches that are involved in merging. But you might want to look at the common ancestor as well. A 3-way merging tool helps you in viewing and comparing the conflicts with your remote ancestor branch (master, for instance). Please download any 3-way merge tool before moving ahead.

**Note:** WinMerge is not a 3-way merge tool. I will be using [DiffMerge][2] here.

So the next time you see any merge conflict, please follow the steps below:

Type **git mergetool**. You will see the default merge tool candidates.

![][3]

You might feel relaxed to see that GIT is offering couple of tools for identifying conflicts. Unfortunately, they all are command-line based tools so we will be adding a visual tool to it, and next time when you write a mergetool command, you will see your own visual tool opened up with the file.

You might get a hint that we need to override the ‘mergetool’ so go ahead and open your .gitconfig file present in C:\Users{user-name}\ (That’s for Windows 7 by the way). You will find tags like [gui], [user], etc. Add following lines to it:

``` 


[merge]

tool = diffmerge

[mergetool "diffmerge"]

cmd = git-merge-diffmerge-wrapper.sh "$PWD/$LOCAL" "$PWD/$REMOTE" "$PWD/$BASE"


 ```

You can specify the name of the mergetool you are using inside the [mergetool] tag, but make sure it matches the ‘tool’ property of [merge] tag. You can see [mergetool] requires git-merge-diffmerge-wrapper.sh. [Download it][4] and paste it inside {GIT_INSTALLATION_FOLDER}\cmd. Please change the extension of the file to .sh. You should have something like below:

![][5]

Also, please add _{GIT_INSTALLATION_FOLDER}\cmd_ in your PATH.

You can enter the path of the executable of the diff tool you wish to use in _git-merge-diffmerge-wrapper.sh._

And you are done! Go back to your GIT Bash and re-enter **git mergetool**. You will see the name of your diff tool.

![][6]

Once you hit ENTER, you can see your diff tool opening the file with three views. I dint get a way to find the branch names involved in the merging so I am referring the title of the views as CURRENT, BRANCH BEING MERGED and ANCESTOR. **CURRENT** – the branch accepting the merge, **BRANCH BEING MERGED** – as the name says and **ANCESTOR** – the remote copy of the file on origin.

![][7]

If you have more than 1 file with conflicts, then your diff tool will be opened for each file. Once you fix the conflict, you can save the file and close the tool. You can then ADD and COMMIT the files.

Hope you can now indentify merge conflicts easily.

   [1]: http://xebee.xebia.in/2011/05/15/analysing-git-branching-model-with-kanban/ (Analysing GIT Branching model with Kanban)
   [2]: http://www.sourcegear.com/diffmerge/downloads.php
   [3]: http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-default-300x57.png
   [4]: http://xebee.xebia.in/wp-content/uploads/2011/09/git-merge-diffmerge-wrapper.txt
   [5]: http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-wrapper-location-300x65.png (mergetool-wrapper-location)
   [6]: http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-custom-tool-300x51.png (mergetool-custom-tool)
   [7]: http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-views-221x300.png (mergetool-views)