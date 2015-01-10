---
layout: post
header-img: img/default-blog-pic.jpg
author: rnagpal
description: 
post_id: 5653
created: 2010/10/28 09:40:58
created_gmt: 2010/10/28 04:40:58
comment_status: open
---

# Working with GeoLocations

<p>Are you looking for an API that can perform the following GeoLocation functions
<div style="text-align: left; margin-left: 130px; float: center;">
<ul>
    <li>Find distance between two points</li>
    <li>Find extreme points i.e. points with min/max latitude and longitude from a center point, for a given distance</li>
    <li>Find point of interest near you</li>
</ul>
</div>
These days we use GeoLocation related services in almost all the applications and sometimes we don't want to be dependent on third party GeoLocation API's for doing some simple operations as mentioned above. For these purposes I tried to use sample code available at  different sites, but some of them gave wrong results and some of them did not suit our requirements. So, I decided to write it myself.</p>
<!--more-->

<p>In this blog I am not going to explain the trigonometric formulae for performing the above operations, but will present you with Java code that you can use. Before I show the code for performing the above listed operations, I  would like to explain some basic terms.</p>
<p><strong>Latitude</strong>- location of a place on Earth (or other planetary body) north or south of the equator. <strong>Lines of Latitude</strong> are the imaginary horizontal lines running east-to-west (or west to east) on maps either north or south of the equator ranging from 0° at the equator to +90° for the North Pole and −90° for the South Pole</p>
<p><strong>Longitude </strong>- An east-west measurement of position on Earth, measured from a vertical plane running through the polar axis and the prime meridian, recorded in degrees, minutes and seconds ranging from 0 degrees at the prime meridian to 180 degrees west and east.</p>
<p><strong>Geocoding </strong>- process of finding associated geographic coordinates( latitude and longitude) from other geographic data, such as street addresses, city name, state or zip codes( postal codes)</p>
<p><strong>Reverse Geocoding </strong>- finding an associated textual location such as a street address, city name ,state, zip code etc from geographic coordinates.</p>
<p>To perform the above mentioned operations like finding distance between points, points in a given radius etc you need latitude and longitude of the center point. As clear from the geocoding definition, you can find latitude and longitude corresponding to an address by using any third party Geocoding API. I have used google geocoding api's which give very good results.
<ol>
    <li><strong>Distance between two Points : </strong> Using the below function you can find distance between two points.
[sourcecode language="java"]
private static double getDistanceBetweenPoints(Point p1, Point p2,
           String unit) {
double theta = p1.longitude - p2.longitude;
double dist = Math.sin(deg2rad(p1.latitude))
        * Math.sin(deg2rad(p2.latitude))
        + Math.cos(deg2rad(p1.latitude))
        * Math.cos(deg2rad(p2.latitude)) * Math.cos(deg2rad(theta));
dist = Math.acos(dist);
dist = rad2deg(dist);
dist = dist * 60 * 1.1515;
if (unit.equals(&quot;K&quot;)) {
     dist = dist * 1.609344;
} else if (unit.equals(&quot;M&quot;)) {
     dist = dist * 0.8684;
}
     return (dist);
}
[/sourcecode]</p>
<p>The distance returned by this method depends upon the units passed. If you want to get distance in Kilometers pass "K" as the argument or if you want distance in Miles pass "M".</li>
    <li><strong>Extreme points :</strong> To find the extreme points you need to know the maximum latitude and longitude difference in degrees corresponding to distance. Using these differences you can determine the maximum and minimum, latitude and longitude from center point. You just have to add or subtract these latitude/longitude differences from the center point and you will get the extreme points.
Latitude difference for a distance from a point
[sourcecode language="java"]
    private static double getExtremeLatitudesDiffForPoint(Point p1,
            double distance) {
        double latitudeRadians = distance / EARTH_RADIUS_KM;
        double diffLat = rad2deg(latitudeRadians);
        return diffLat;
    }
[/sourcecode]</p>
<p>Longitude difference for a distance from a point</p>
<p>[sourcecode language="java"]
    private static double getExtremeLongitudesDiffForPoint(Point p1,
            double distance) {
        double lat1 = p1.latitude;
        lat1 = deg2rad(lat1);
        double longitudeRadius = Math.cos(lat1) * EARTH_RADIUS_KM;
        double diffLong = (distance / longitudeRadius);
        diffLong = rad2deg(diffLong);
        return diffLong;
    }
[/sourcecode]</p>
<p>Extreme Points</p>
<p>[sourcecode language="java"]
    private static Point[] getExtremePointsFrom(Point point, Double distance) {
        double longDiff = getExtremeLongitudesDiffForPoint(point, distance);
        double latDiff = getExtremeLatitudesDiffForPoint(point, distance);
        Point p1 = new Point(point.latitude - latDiff, point.longitude
                - longDiff);
        p1 = validatePoint(p1);
        Point p2 = new Point(point.latitude + latDiff, point.longitude
                + longDiff);
        p2 = validatePoint(p2);</p>
<pre><code>    return new Point[] { p1, p2 };
}
</code></pre>
<p>[/sourcecode]</p>
<p>This method will return an array of two extreme points corresponding to center point and the distance from the center point. These extreme points are the points with max/min latitude and longitude.</li>
    <li><strong>Find point of interest near you :</strong> If you are using MySql you can use the below query to find all the points within a particular radius.Suppose you have an address table which has all the points of interest, along with latitude and longitude for each point, query to find all the points inside a given radius is [sourcecode language="sql"]
select <em>,3956</em>2<em>ASIN(SQRT(POWER(SIN((37.804372-abs(dest.latitude))</em>pi()/180/2 ), 2) + COS(37.804372<em>pi()/180)</em>COS(abs(dest.latitude) <em>pi()/180)</em>POWER(SIN((-122.270803-dest.longitude)*pi()/180/2 ),2))) as distance from address dest having distance &lt;100 order by distance;
[/sourcecode]</p>
<p>Let us assume you have some points present in your database and you want to find all the points within 10 miles of distance from the center point, you can execute the above query against the database and get the relevant points. The points returned will lie between a circle of 10 miles radius drawn from the center point.
<a rel="attachment wp-att-5166" href="http://xebee.xebia.in/?attachment_id=5166"><img class="size-full wp-image-5166 alignleft" title="within-radius" src="http://xebee.xebia.in/wp-content/uploads/2010/10/within-radius3.png" alt="" width="400" height="272" /></a>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
The execution of the above query will be time consuming if you have lot of points in your database, as it has lot of operations in it. In this case it would be advisable to find the extreme points and use the latitude/longitude of these points to get all the points lying within the latitude and longitude ranges of extreme points i.e. find the points between the max and min latitude/longitude. Now you will have a simple 'between' clause in your sql query, rather than the big computation discussed in previous case.</p>
<p><a rel="attachment wp-att-5245" href="http://xebee.xebia.in/?attachment_id=5245"><img class="alignleft size-full wp-image-5245" title="within-extremepoints" src="http://xebee.xebia.in/wp-content/uploads/2010/10/within-extremepoints.png" alt="" width="393" height="336" /></a>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
Now using the extreme points you will get all the points that are in the square, with side length of square equal to distance and is located at the center from which you want your points. You can now find the distance of  these points from center and filter out the results whose distance is greater than given distance.</li>
</ol>
I have also attached the java file which contains all these functions.
<a href="http://www.sendspace.com/file/yv0lki" target="_blank">GeoLocationService.java</a></p>

## Comments

**[ken](#3175 "2010-11-09 18:12:27"):** public double distance(double lat1, double lon1, double lat2, double lon2, String unit) { double theta = lon1 - lon2; double dist = Math.sin(deg2rad(lat1)) * Math.sin(deg2rad(lat2)) + Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) * Math.cos(deg2rad(theta)); dist = Math.acos(dist); dist = rad2deg(dist); dist = dist * 60 * 1.1515; if (unit == "K") { dist = dist * 1.609344; } else if (unit == "N") { dist = dist * 0.8684; } return (dist); } N is nautical mile , if u just enter an empty string default distance is in miles

**[Karthick](#5275 "2011-02-07 07:47:08"):** Excellent, explanation/code. Perfectly matches what I was looking for. Thanks for the blog entry. Whats the license in which the code is released? It is free to use? Thanks, Karhtick

**[Robin Nagpal](#5278 "2011-02-09 08:51:50"):** Yes, code is free to use ;)

**[Ali](#5560 "2011-05-09 13:37:08"):** Dear sir, i would like to thank you for this code, which was a helpful to work with wikimapia API to calculate the min and max points in order to fetch near places BR,

**[Robin Nagpal](#5562 "2011-05-09 17:16:25"):** ohh good to hear that I am able to help somebody

**[Sanketh](#5461 "2011-04-13 10:11:22"):** Hi, do u have any idea relating to android? am creating an android app in that i want to fetch lat long values from the google map of my id..

**[Narinder Kumar](#5596 "2011-05-26 02:21:36"):** Hi Robin, Excellent article. Very helpful. I was working with geocoded data and struggling to find something which I can readily use and your code sample just worked out of the box. I have another problem at hand perhaps you have a response for the same as well. Given a LatLong (Point) and a radius, I would like to find various points on the desired Circle, let's say LatLong of every point on the circumference of the circle at some degree (example 0 deg, 36 deg, 72 deg so on till 360 deg) Again many thanks. Best Regards Narinder

**[Robin Nagpal](#5550 "2011-05-04 16:41:07"):** The code will surely be similar as it is just mathematics and no functionality. 2+2 =4 will always look similar what ever you do. And yes I don't think thre can be any licenses to these type of code snippets

**[snowb0y](#5548 "2011-05-04 03:22:04"):** Your code looks very similar to this. http://www.zipcodeworld.com/samples/distance.java.html .. without the license.

**[CH NAGA SUDHA](#5928 "2011-09-20 13:15:54"):** Thanku sir

**[Khantil patel](#7751 "2012-02-24 11:41:40"):** Hi, thanx for this blog your effort have helped me big time.

**[Dheeru Mundluru](#9090 "2012-06-23 04:31:48"):** Hi Robin, Firstly, thanks for posting your code. However, I am not 100% sure about the accuracy of the miles that is being returned by your method. Following is one test case and your solution seems to be off by almost 50 miles (verified on Google maps - however we cannot probably rely on that as it uses driving distance). I also tried for another sample and it was off by 4 miles. I provided the modified version of the method below and it is only for miles. I have been using this formula (spherical law of cosines) for a long time although in my case it was always coded in SQL (only now I had a need to put it in pure Java and that's how I stumbled on your code). Multiplying with 3963 (earth radius) is standard. Point p1 = new Point(37.3501, -121.9854); Point p2 = new Point(33.6751, -117.7339); Below method gave: 349.0701120865378 Your formula gave: 302.79328035707783 private static double getDistanceBetweenPoints(Point p1, Point p2) { double theta = p1.longitude - p2.longitude; double dist = Math.sin(deg2rad(p1.latitude)) * Math.sin(deg2rad(p2.latitude)) \+ Math.cos(deg2rad(p1.latitude)) * Math.cos(deg2rad(p2.latitude)) * Math.cos(deg2rad(theta)); dist = Math.acos(dist); return dist * 3963; } If possible, pls post your thoughts on this. Thanks, -Dheeru

