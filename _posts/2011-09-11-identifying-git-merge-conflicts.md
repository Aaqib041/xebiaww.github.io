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

<p>One of the pain points in GIT <strong>was</strong> resolving merge conflicts. I have highlighted ‘was’ because at the end of this blog you will find your life easy the next time you encounter merge conflicts. Working with GIT in agile development increases the probability of merge conflicts, and the reason behind is straight forward – Parallel Development.  As I mentioned in my earlier <a title="Analysing GIT Branching model with Kanban" href="http://xebee.xebia.in/2011/05/15/analysing-git-branching-model-with-kanban/" target="_blank">blog</a>, there are lot of advantages of using GIT branching model. But one of the pain points I mentioned was resolving merge conflicts. One has to manually open the files and identify conflicts. In this blog, I will share how you can configure and open a 3-way merging tool to identify your merge conflicts. <!--more--></p>
<p>Why a 3-way merging tool? You always have two branches that are involved in merging. But you might want to look at the common ancestor as well. A 3-way merging tool helps you in viewing and comparing the conflicts with your remote ancestor branch (master, for instance). Please download any 3-way merge tool before moving ahead.</p>
<p><strong>Note:</strong> WinMerge is not a 3-way merge tool. I will be using <a href="http://www.sourcegear.com/diffmerge/downloads.php" target="_blank">DiffMerge</a> here.</p>
<p>So the next time you see any merge conflict, please follow the steps below:</p>
<p>Type <strong>git mergetool</strong>. You will see the default merge tool candidates.</p>
<p><img class="alignleft" src="http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-default-300x57.png" alt="" width="300" height="57" /></p>
<p>You might feel relaxed to see that GIT is offering couple of tools for identifying conflicts. Unfortunately, they all are command-line based tools so we will be adding a visual tool to it, and next time when you write a mergetool command, you will see your own visual tool opened up with the file.</p>
<p>You might get a hint that we need to override the ‘mergetool’ so go ahead and open your .gitconfig file present in C:\Users{user-name}\ (That’s for Windows 7 by the way). You will find tags like [gui], [user], etc. Add following lines to it:</p>
<p>[code]</p>
<p>[merge]</p>
<p>tool = diffmerge</p>
<p>[mergetool &quot;diffmerge&quot;]</p>
<p>cmd = git-merge-diffmerge-wrapper.sh &quot;$PWD/$LOCAL&quot; &quot;$PWD/$REMOTE&quot; &quot;$PWD/$BASE&quot;</p>
<p>[/code]</p>
<p>You can specify the name of the mergetool you are using inside the [mergetool] tag, but make sure it matches the ‘tool’ property of [merge] tag. You can see [mergetool] requires git-merge-diffmerge-wrapper.sh. <a href="http://xebee.xebia.in/wp-content/uploads/2011/09/git-merge-diffmerge-wrapper.txt" target="_blank">Download it</a> and paste it inside {GIT_INSTALLATION_FOLDER}\cmd. Please change the extension of the file to .sh. You should have something like below:</p>
<p><a rel="attachment wp-att-9621" href="http://xebee.xebia.in/2011/09/11/identifying-git-merge-conflicts/mergetool-wrapper-location/"><img class="size-medium wp-image-9621 alignleft" title="mergetool-wrapper-location" src="http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-wrapper-location-300x65.png" alt="" width="300" height="65" /></a></p>
<p>Also, please add <em>{GIT_INSTALLATION_FOLDER}\cmd</em> in your PATH.</p>
<p>You can enter the path of the executable of the diff tool you wish to use in <em>git-merge-diffmerge-wrapper.sh.</em></p>
<p>And you are done! Go back to your GIT Bash and re-enter <strong>git mergetool</strong>. You will see the name of your diff tool.</p>
<p><a rel="attachment wp-att-9626" href="http://xebee.xebia.in/2011/09/11/identifying-git-merge-conflicts/mergetool-custom-tool/"><img class="size-medium wp-image-9626 alignleft" title="mergetool-custom-tool" src="http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-custom-tool-300x51.png" alt="" width="300" height="51" /></a></p>
<p>Once you hit ENTER, you can see your diff tool opening the file with three views. I dint get a way to find the branch names involved in the merging so I am referring the title of the views as CURRENT, BRANCH BEING MERGED and ANCESTOR. <strong>CURRENT</strong> – the branch accepting the merge, <strong>BRANCH BEING MERGED</strong> – as the name says and <strong>ANCESTOR</strong> – the remote copy of the file on origin.</p>
<p><a rel="attachment wp-att-9627" href="http://xebee.xebia.in/2011/09/11/identifying-git-merge-conflicts/mergetool-views/"><img class="alignleft size-medium wp-image-9627" title="mergetool-views" src="http://xebee.xebia.in/wp-content/uploads/2011/09/mergetool-views-221x300.png" alt="" width="221" height="300" /></a></p>
<p>If you have more than 1 file with conflicts, then your diff tool will be opened for each file. Once you fix the conflict, you can save the file and close the tool. You can then ADD and COMMIT the files.</p>
<p>Hope you can now indentify merge conflicts easily.</p>