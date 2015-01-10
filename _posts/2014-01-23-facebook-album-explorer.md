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

As a followup of my earlier blog [Developing Application on facebook : Facebook basic setup][1], I will show you, how you can explore the album of the logged in user. After Authorization, we can query for album data from Facebook.(Make sure you have taken **user_photos** permission while authorizing the app)Below is the code for querieng album data from Facebook: [sourcecode language="javascript"] FB.api('me?fields=albums.fields(photos,name)', function(response) { if (response.albums) {//check if data is recieved //Process data as per your choice } else { alert(&quot;you have not authorized the app to view your images&quot;); } }); [/sourcecode] A sample response is given below: [sourcecode language="javascript"] "id":"USER_ID", "albums":{ "data":[ { "name":"ALBUM_NAME", "id":"ALBUM_ID", "created_time":"2010-12-04T13:56:08+0000", "photos":{ "data":[ { "id":"PHOTO_ID", "from":{ "name":"FROM_NAME", "id":"FROM_USER_ID" }, "name":"NAME_OF_PHOTO", "picture":"http://photos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/1455866_1020182615152894gf2_815648423_s.jpg", "source":"http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/s720x720/1455866_1020182gfgf6151528942_815648423_n.jpg", "height":405, "width":720, "images":[ { "height":540, "width":960, "source":"http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/1455866_10201826vdf151528942_815648423_n.jpg" }, { "height":405, "width":720, "source":"http://sphotos-h.ak.fbcdn.net/hphotos-ak-prn2/t1/q77/s720x720/1455df866_10201826151528942_815648423_n.jpg" },] }]} [/sourcecode]

You can process this data according to your choice. For this blog First page will show Album List and then after selecting an album you will be shown all the photos in it.   
To make this working you need to download  and add following css to your HTML Page [sourcecode language="css"] .album-header { background-color: #6D84B4; color: white; border-radius: 5px 5px 0 0; }

.albumModalFirstPageIco { border: 10px solid #E4E4E4; /_ float: left; _/ height: 150px; margin: 3px 7px 0 0; width: 170px; }

.active_img { border: 10px solid #F2875E; }

.albumModalFirstPageIco:HOVER { border: 10px solid #F2875E; }

.text_class { bottom: 20px; color: #FFFFFF; font-size: 30px; font-weight: bold; left: 0; position: absolute; text-shadow: 0 0 3px rgba(0, 0, 0, 0.75); z-index: 3; }

.text_class_facebook { bottom: 0; color: #FFFFFF; font-size: 15px; font-weight: bold; left: 15px; position: absolute; right: 15px; text-shadow: 0 0 3px rgba(0, 0, 0, 0.75); z-index: 3; }

.div_element { overflow: hidden; position: relative; /_ float: left; _/ display: inline; }

.header_border { height: 35px; background-color: #E4E4E4; }

.bottom_border { height: 50px; background-color: #E4E4E4; }

.bottom_buttton { cursor: pointer; float: right; margin: 8px 15px 0 0; padding: 5px; }

.loadingImg { background: url("../images/loading.gif") no-repeat center; background-size: 30px 30px; }

.loadingDiv { position: fixed; z-index: 1000; top: 0; left: 0; height: 100%; width: 100%; background: rgba(255, 255, 255, .8) url('http://i.stack.imgur.com/FhHRx.gif') 50% 50% no-repeat; } [/sourcecode]

loading.gif can be downloaded from [here][2]. HTML can be used as below: [sourcecode language="html"] <div id="albumExplorerDiv"> <p>Click below to Explore your Album.</p> <p> <a class="btn btn-primary btn-large explore-btn"> Explore Album </a> </p> <div id="showPic"></div> </div> <div id="albumModal" class="modal fade" tabindex="-1" role="dialog" style="display: none; width: 700px;" aria-labelledby="myModalLabel" aria-hidden="true"> <div class="modal-header album-header"> <button class="close" aria-hidden="true" data-dismiss="modal" type="button">×</button> <h3 id="myModalLabel">Choose from photos</h3> </div> <div class="header_border"></div> <div id="albumModalBody" class="modal-body" style="text-align: center"></div> <div class="modal-footer"> <button class="btn btn-primary btn_close" data-dismiss="modal" aria-hidden="true">Close</button> <button class="btn btn-primary modal_back_btn hide">Back</button> </div> </div> [/sourcecode] and javascript for the above HTML as : [sourcecode language="javascript"] $(document).ready(function() { $(".albumExplorer-nav").addClass("active"); loadFacebookScripts("YOUR_APP_ID"); AssignToSendButton(); }); function AssignToSendButton() { $(".explore-btn").on("click", function() { $('#albumModal').modal('toggle'); $("#albumModalBody").html($("#albumModalFirstPage").html()); getAlbumFromFacebook(); }); } var modalStage = "first"; var facebookAlbumHtml ; function getAlbumFromFacebook() { $("#albumModalBody").html(""); $("#albumModalBody").addClass("loadingImg"); FB .api( 'me?fields=albums.fields(photos,name)', function(response) { if (response.albums) { albumData = response.albums.data; var albumsHtml = '<div class="albumModalFirstPageClass">'; for ( var i = 0; i < albumData.length; i++) { if (albumData[i].photos) { albumsHtml = albumsHtml \+ '<div class="div_element">' \+ '<a href="#" onclick="showAlbumPics(\'' \+ albumData[i].id \+ '\')"><img src="'+albumData[i].photos.data[0].source+'" class="albumModalFirstPageIco" />' \+ '<div class="text_class_facebook">' \+ albumData[i].name \+ "</div></a></div>"; } } albumsHtml = albumsHtml + '</div>'; facebookAlbumHtml = albumsHtml; $("#albumModalBody").html(albumsHtml); $("#albumModalBody").removeClass("loadingImg"); modalStage = "first"; $(".modal_back_btn").addClass("hide"); } else { alert("you have not authorized the app to view your images"); } }); }
    
    
    function showAlbumPics(id) {
        $(&quot;#albumModalBody&quot;).addClass(&quot;loadingImg&quot;);
        $(&quot;#albumModalBody&quot;).html(&quot;&quot;);
        selectedAlbumId = id;
    

   [1]: http://xebee.xebia.in/index.php/2013/11/20/developing-application-on-facebook-facebook-basic-setup/ (Developing Application on facebook : Facebook basic setup)
   [2]: http://xebee.xebia.in/wp-content/uploads/2014/01/loading.gif (here)