---
layout: post
header-img: img/default-blog-pic.jpg
author: GauravS
description: 
post_id: 8899
created: 2011/05/15 20:45:02
created_gmt: 2011/05/15 15:45:02
comment_status: open
---

# Analysing GIT Branching model with Kanban

We have been working with GIT branching model for a long time now. [GIT][1] allows you to maintain different branches in your codebase. It is a developer's choice when to make/merge branches. We, as a team decided on certain guidelines as to when make/merge branches. Apart from the user story specific branches, we had _Development _and _Master_. The concept of _Development _branch was introduced to keep our _Master _stable and be in a state to give release-ready builds. All the user story branches are merged into _Development_. 

Just to have an idea, here is how our Kanban board looks like -

![Kanban Board][2]

So for each story, we made a remote branch in GIT. To make is simpler and globalized, we named all branches as US-XXXX. The user story branches were created from _Development_, so that we always have the updated code. The developers, either individually or in a pair worked on user stories. The story moves from left to right. Once it reaches the _Acceptance Done_ column, it's time to merge them to a standard branch named _Developmen_t. Finally, the _Development _branch is being merged into _Master _whenever there is a Release.

Over the time we realized there are some good things that comes with this approach. 

  1. **_Master _always remain stable:** With this approach, we were sure that we won't make any commit directly on master. We have enough buffer to check if our commit is not breaking any of the tests.
  2. **Bugs:** Whenever there is a new feature in the product, it is added as a user story. With the above approach we make a dedicated branch for that user story. So for any bug related to the user story, we always know which branch to work upon. So the bug can be worked upon in isolation.
  3. **Test cases for each user story: **Stories are not moved in _Development Done_ untill there are Unit Test cases for it. So this way we make sure that the test cases are present on the branch only. The branch is fully testable before moving into the next column.
  4. **Parallel development**: Developers can work on work on their user story in parallel to others, which is independent of other changes/modifications in the application.
Apart from these positive points, there were some issues also with this approach. 
  1. **Story might get blocked:** This is the most common problem we faced while following this approach. A practical use case is - Developer A working on a user story US-AAAA, and the story is currently in _Development_ column. Developer A does heavy refactoring which affects all stories related to US-AAAA. A related story say US-BBBB is being picked by Developer B. The refactoring done by Developer A is now required by Developer B. But Developer B won't have this refactoring untill the story reaches _Acceptance Done_ column. Because after this column, the story will be merged into _Development_. So for that particular time, US-AAAA remains blocked.
  2. **Merge conflicts:** Whenever a story is being merged with 'Development', we often face merge conflicts. The reason is pretty clear - All stories have their own branches, which may have common files/classes being modified. So when they are merged, GIT is not able to able auto-merge those changes.
We are using this approach for a good amount of time now, and we feel that the good things that we are getting with this approach is making our development smoother and efficient. Although we do come across some hurdles with this approach, but that's an opportunity to learn and adapt!

   [1]: http://git-scm.com/
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/05/kanban-board.png

## Comments

**[Gaurav Srivastava](#5606 "2011-06-01 18:43:23"):** Thanks for your suggestion Iwein. True, we can always cherry pick the desired commit. It's been a long time that we are working with GIT, and gradually we are learning about these tips.

**[Gaurav Srivastava](#5590 "2011-05-23 17:09:25"):** Thanks Vivek and Martin. Martin, Yes even i agree with you on this. While creating stories we can try to keep the related ones together. So we do not create multiple branches for it, and thus we can avoid blocking of stories.

**[Vivek](#5574 "2011-05-16 14:12:03"):** Nice Summarization!

**[Martin](#5577 "2011-05-17 15:05:42"):** Good description of our development methodology. One thing that will relieve issue 1 is to try to minimize the number of dependent stories in progress at once.

**[Iwein Fuld](#5593 "2011-05-24 22:35:53"):** Cool story, thanks! One remark on the strategy that might help you: Downside 1 is implying the assumption that you can only get a refactoring done in US-AAAA into US-AAAB after US-AAAA is merged into Development, but this assumption is false. In fact you can get any commit from any branch on any other branch without merging. To do this you can use cherry picking (http://www.kernel.org/pub/software/scm/git/docs/git-cherry-pick.html). The awesomeness you get with this is that git understands that the cherry you picked doesn't have to be merged downstream, saving you loads of merge conflicts. This is one of the fundamental changes in thinking when you move from svn to git. You don't think: "that branch has the changes I want", but you think "that commit has the changes I want" and you just go grab it.

