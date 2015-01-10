---
layout: post
header-img: img/default-blog-pic.jpg
author: pgupta
description: 
post_id: 17857
created: 2014/01/23 12:34:20
created_gmt: 2014/01/23 07:34:20
comment_status: open
---

# Facebook Album Explorer

<p>As a followup of my earlier blog <a href="http://xebee.xebia.in/index.php/2013/11/20/developing-application-on-facebook-facebook-basic-setup/" title="Developing Application on facebook : Facebook basic setup" target="_blank">Developing Application on facebook : Facebook basic setup</a>, I will show you, how you can explore the album of the logged in user.
After Authorization, we can query for album data from Facebook.(Make sure you have taken <strong>user_photos</strong> permission while authorizing the app)Below is the code for querieng album data from Facebook:
[sourcecode language="javascript"]
FB.api('me?fields=albums.fields(photos,name)',
 function(response) {
   if (response.albums) {//check if data is recieved
       //Process data as per your choice
      } else {
    alert(&amp;quot;you have not authorized the app to view your images&amp;quot;);
      }
   });
[/sourcecode]
A sample response is given below:
[sourcecode language="javascript"]
&quot;id&quot;:&quot;USER_ID&quot;,
   &quot;albums&quot;:{
      &quot;data&quot;:[
         {
            &quot;name&quot;:&quot;ALBUM_NAME&quot;,
            &quot;id&quot;:&quot;ALBUM_ID&quot;,
            &quot;created_time&quot;:&quot;2010-12-04T13:56:08+0000&quot;,
            &quot;photos&quot;:{
               &quot;data&quot;:[
                  {
                     &quot;id&quot;:&quot;PHOTO_ID&quot;,
                     &quot;from&quot;:{
                        &quot;name&quot;:&quot;FROM_NAME&quot;,
                        &quot;id&quot;:&quot;FROM_USER_ID&quot;
                     },
                     &quot;name&quot;:&quot;NAME_OF_PHOTO&quot;,
                     &quot;picture&quot;:&quot;http://photos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/1455866_1020182615152894gf2_815648423_s.jpg&quot;,
                     &quot;source&quot;:&quot;http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/s720x720/1455866_1020182gfgf6151528942_815648423_n.jpg&quot;,
                     &quot;height&quot;:405,
                     &quot;width&quot;:720,
                     &quot;images&quot;:[
                        {
                           &quot;height&quot;:540,
                           &quot;width&quot;:960,
                           &quot;source&quot;:&quot;http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/1455866_10201826vdf151528942_815648423_n.jpg&quot;
                        },
                        {
                           &quot;height&quot;:405,
                           &quot;width&quot;:720,
                           &quot;source&quot;:&quot;http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/q77/s720x720/1455df866_10201826151528942_815648423_n.jpg&quot;
                        },]
                }]}
[/sourcecode]</p>
<p>You can process this data according to your choice. For this blog First page will show Album List and then after selecting an album you will be shown all the photos in it. <br />
To make this working you need to download <a href="http://getbootstrap.com/2.3.2/" title="twtter bootstrap"></a> and add following css to your HTML Page
[sourcecode language="css"]
.album-header {
    background-color: #6D84B4;
    color: white;
    border-radius: 5px 5px 0 0;
}</p>
<p>.albumModalFirstPageIco {
    border: 10px solid #E4E4E4;
    /<em>  float: left; </em>/
    height: 150px;
    margin: 3px 7px 0 0;
    width: 170px;
}</p>
<p>.active_img {
    border: 10px solid #F2875E;
}</p>
<p>.albumModalFirstPageIco:HOVER {
    border: 10px solid #F2875E;
}</p>
<p>.text_class {
    bottom: 20px;
    color: #FFFFFF;
    font-size: 30px;
    font-weight: bold;
    left: 0;
    position: absolute;
    text-shadow: 0 0 3px rgba(0, 0, 0, 0.75);
    z-index: 3;
}</p>
<p>.text_class_facebook {
    bottom: 0;
    color: #FFFFFF;
    font-size: 15px;
    font-weight: bold;
    left: 15px;
    position: absolute;
    right: 15px;
    text-shadow: 0 0 3px rgba(0, 0, 0, 0.75);
    z-index: 3;
}</p>
<p>.div_element {
    overflow: hidden;
    position: relative;
    /<em>  float: left; </em>/
    display: inline;
}</p>
<p>.header_border {
    height: 35px;
    background-color: #E4E4E4;
}</p>
<p>.bottom_border {
    height: 50px;
    background-color: #E4E4E4;
}</p>
<p>.bottom_buttton {
    cursor: pointer;
    float: right;
    margin: 8px 15px 0 0;
    padding: 5px;
}</p>
<p>.loadingImg {
    background: url(&quot;../images/loading.gif&quot;) no-repeat center;
    background-size: 30px 30px;
}</p>
<p>.loadingDiv {
    position: fixed;
    z-index: 1000;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background: rgba(255, 255, 255, .8)
        url('http://i.stack.imgur.com/FhHRx.gif') 50% 50% no-repeat;
}
[/sourcecode]</p>
<p>loading.gif can be downloaded from <a href="http://xebee.xebia.in/wp-content/uploads/2014/01/loading.gif" title="here" target="_blank">here</a>.
HTML can be used as below:
[sourcecode language="html"]
&lt;div id=&quot;albumExplorerDiv&quot;&gt;
    &lt;p&gt;Click below to Explore your Album.&lt;/p&gt;
    &lt;p&gt;
        &lt;a class=&quot;btn btn-primary btn-large explore-btn&quot;&gt; Explore Album &lt;/a&gt;
    &lt;/p&gt;
    &lt;div id=&quot;showPic&quot;&gt;&lt;/div&gt;
&lt;/div&gt;
&lt;div id=&quot;albumModal&quot; class=&quot;modal fade&quot; tabindex=&quot;-1&quot; role=&quot;dialog&quot;
    style=&quot;display: none; width: 700px;&quot; aria-labelledby=&quot;myModalLabel&quot;
    aria-hidden=&quot;true&quot;&gt;
    &lt;div class=&quot;modal-header album-header&quot;&gt;
        &lt;button class=&quot;close&quot; aria-hidden=&quot;true&quot; data-dismiss=&quot;modal&quot;
            type=&quot;button&quot;&gt;×&lt;/button&gt;
        &lt;h3 id=&quot;myModalLabel&quot;&gt;Choose from photos&lt;/h3&gt;
    &lt;/div&gt;
    &lt;div class=&quot;header_border&quot;&gt;&lt;/div&gt;
    &lt;div id=&quot;albumModalBody&quot; class=&quot;modal-body&quot; style=&quot;text-align: center&quot;&gt;&lt;/div&gt;
    &lt;div class=&quot;modal-footer&quot;&gt;
        &lt;button class=&quot;btn btn-primary btn_close&quot; data-dismiss=&quot;modal&quot;
            aria-hidden=&quot;true&quot;&gt;Close&lt;/button&gt;
        &lt;button class=&quot;btn btn-primary modal_back_btn hide&quot;&gt;Back&lt;/button&gt;
    &lt;/div&gt;
&lt;/div&gt;
[/sourcecode]
and javascript for the above HTML as :
[sourcecode language="javascript"]
$(document).ready(function() {
        $(&quot;.albumExplorer-nav&quot;).addClass(&quot;active&quot;);
        loadFacebookScripts(&quot;YOUR_APP_ID&quot;);
        AssignToSendButton();
    });
    function AssignToSendButton() {
        $(&quot;.explore-btn&quot;).on(&quot;click&quot;, function() {
            $('#albumModal').modal('toggle');
            $(&quot;#albumModalBody&quot;).html($(&quot;#albumModalFirstPage&quot;).html());
            getAlbumFromFacebook();
        });
    }
    var modalStage = &quot;first&quot;;
    var facebookAlbumHtml ;
    function getAlbumFromFacebook() {
        $(&quot;#albumModalBody&quot;).html(&quot;&quot;);
        $(&quot;#albumModalBody&quot;).addClass(&quot;loadingImg&quot;);
        FB
                .api(
                        'me?fields=albums.fields(photos,name)',
                        function(response) {
                            if (response.albums) {
                                albumData = response.albums.data;
                                var albumsHtml = '&lt;div class=&quot;albumModalFirstPageClass&quot;&gt;';
                                for ( var i = 0; i &lt; albumData.length; i++) {
                                    if (albumData[i].photos) {
                                        albumsHtml = albumsHtml
                                                + '&lt;div class=&quot;div_element&quot;&gt;'
                                                + '&lt;a href=&quot;#&quot; onclick=&quot;showAlbumPics(\''
                                                + albumData[i].id
                                                + '\')&quot;&gt;&lt;img src=&quot;'+albumData[i].photos.data[0].source+'&quot;  class=&quot;albumModalFirstPageIco&quot; /&gt;'
                                                + '&lt;div class=&quot;text_class_facebook&quot;&gt;'
                                                + albumData[i].name
                                                + &quot;&lt;/div&gt;&lt;/a&gt;&lt;/div&gt;&quot;;
                                    }
                                }
                                albumsHtml = albumsHtml + '&lt;/div&gt;';
                                facebookAlbumHtml = albumsHtml;
                                $(&quot;#albumModalBody&quot;).html(albumsHtml);
                                $(&quot;#albumModalBody&quot;).removeClass(&quot;loadingImg&quot;);
                                modalStage = &quot;first&quot;;
                                $(&quot;.modal_back_btn&quot;).addClass(&quot;hide&quot;);
                            } else {
                                alert(&quot;you have not authorized the app to view your images&quot;);
                            }
                        });
    }</p>
<pre><code>function showAlbumPics(id) {
    $(&amp;quot;#albumModalBody&amp;quot;).addClass(&amp;quot;loadingImg&amp;quot;);
    $(&amp;quot;#albumModalBody&amp;quot;).html(&amp;quot;&amp;quot;);
    selectedAlbumId = id;
</code></pre>