---
layout: post
header-img: img/default-blog-pic.jpg
---

# Getting through Popup Blockers with window.open() in Flex

It was a few years back when I last worked with Javascript. Things have changed significantly in between and one of the things which we people always like is the introduction of popup-blockers in browser world. I hope you remember those old days when it was a nuisance to close all those uninvited windows. Now we are living in a relatively peaceful world. But sometimes we also want to open new windows for legitimate reasons. So when I had a task to open a new browser window on button-click, I discovered the problems posed by popup-blockers. The interesting part is - window.open() behavior can be different for different browsers and popup-blockers.  One of the things which is common amongst all browsers is - popup should open from a user action like button-click, link click etc. Popup-blockers almost always block windows opened without user-actions which looks fine to me as otherwise spammers could open dozens of windows without asking you. However this posed a bit of problem in Flex for me. I wanted to open a window after invoking some backend services. As responses (results from web-services) coming from Flex events are always asynchronous, it was just not possible to open the window after having some backend interaction. I had to change some of my design. Another problem was different browsers behaviors. In some browsers Flex based ExternalInterface.call() method works and on some navigationToURL(). While solving this problem, I came across with a [good link](http://www.mehtanirav.com/2008/11/27/opening-external-links-in-new-window-from-as3) which solves most of the window.open() issues on different browsers. Safari poses yet another challenge when it comes to using window.focus() function. In many of the situations you'd like to open one single named window (important from usability perspective). While using the same window for a new URL, you may want to put focus on exiting window using window.focus() function. Safari doesn't support window.focus() function. I tried to find a suitable alternative but nothing worked except close and open the window again as follows: 
    
    
    var newwindow = '';
    function popitup(url) {
        if (!newwindow.closed && newwindow.location) {
            newwindow.close();
            newwindow=window.open(url,'name'+Math.random(),'height=600,width=800');
        }
        else {
            newwindow=window.open(url,'name','height=600,width=800');
            if (!newwindow.opener) newwindow.opener = self;
        }
        return false;
    }
    

Please note I used a new window name every time I close a window. Again in Safari one cannot reopen the same named window after closing it. I also used following references which were helpful for me. 

  * [Javascript - Cross window scripting](http://www.quirksmode.org/js/croswin.html)
  * [about window.open() in safari](http://www.webmaster-talk.com/javascript-forum/105303-about-window-open-in-safari.html)