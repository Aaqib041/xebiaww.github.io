---
layout: post
header-img: img/default-blog-pic.jpg
author: jpuri
description: 
post_id: 18752
created: 2014/08/02 14:58:33
created_gmt: 2014/08/02 09:58:33
comment_status: open
---

# AngularJS – Role based access on GUI

<p dir="ltr">These days we are busy in coding for frontend in our application and we are primarily using angular for that. A recent requirement was for role based access on GUI. A person can have multiple roles assigned to him and it was needed that a he should be able to access only that portions of GUI for which he is authorized.</p>

<p dir="ltr">We approached to solve the issue by restricting the access of the application's GUI at mainly 2 levels:</p>

<ol>
    <li dir="ltr">
<p dir="ltr">A person should be able access only those pages (or we can say routes) for which he is authorized.</p>
</li>
    <li dir="ltr">
<p dir="ltr">On a page a person should see only those portions for which he is authorized.</p>
</li>
</ol>

<p dir="ltr">To address 1 above we captured <strong>route change event</strong> and on every route change we will check if user is authorized for next route, in case he is not we will show him access denied page.</p>

<pre>$rootScope.$on("$routeChangeStart", function(event, next, current) {
    if(!authService.isUrlAccessibleForUser(next.originalPath))
    $location.path('/authError');
});</pre>

<p><span style="line-height: 1.5em;"><strong>authService</strong> above is a service we have created which has list of the roles user has and the routes each role is authorized for. The function <strong>isUrlAccessibleForUser()</strong> will check if any of the various roles user is assigned can access the given route and will return true or false.</span>
<p dir="ltr">The service will obtain the list of roles for user from the backend and the details of route accessible to each role will be saved a map in the service itself (these details can also be saved in the db and fetched from backend instead).</p>
<!--more-->
<pre>app.factory('authService', function ($http) {</p>
<pre><code>var userRole = []; // obtained from backend
var userRoleRouteMap = {
    'ROLE_ADMIN': [ '/dashboard', '/about-us', '/authError' ],
    'ROLE_USER': [ '/usersettings', '/usersettings/personal', '/authError']
};

return {

    userHasRole: function (role) {
        for (var j = 0; j &amp;lt; userRole.length; j++) {
            if (role == userRole[j]) {
                return true;
            }
        }
        return false;
    },

    isUrlAccessibleForUser: function (route) {
        for (var i = 0; i &amp;lt; userRole.length; i++) {
            var role = userRole[i];
            var validUrlsForRole = userRoleRouteMap[role];
            if (validUrlsForRole) {
                for (var j = 0; j &amp;lt; validUrlsForRole.length; j++) {
                    if (validUrlsForRole[j] == route)
                        return true;
                }
            }
        }
        return false;
    }
};
</code></pre>
<p>});</pre>
<span style="line-height: 1.5em;">This will address point 1 above, to address point 2 we created a directive. The directive was used something like:</span>
<pre>&lt;div my-access=”ROLE_ADMIN”&gt;......&lt;/div&gt;</pre>
<p dir="ltr">The html tag above will be rendered on the page only if the user has ROLE_ADMIN else this html fragment will be removed from the page. The code for the directive is:</p></p>
<pre>.directive('myAccess', ['authService', 'removeElement', function (authService, removeElement) {
    return{
        restrict: 'A',
        link: function (scope, element, attributes) {

            var hasAccess = false;
            var allowedAccess = attributes.myAccess.split(" ");
            for (i = 0; i &lt; allowedAccess.length; i++) {
                if (authService.userHasRole(allowedAccess[i])) {
                    hasAccess = true;
                    break;
                }
            }

            if (!hasAccess) {
                angular.forEach(element.children(), function (child) {
                    removeElement(child);
                });
                removeElement(element);
            }

        }
    }
}]).constant('removeElement', function(element){
    element &amp;&amp; element.remove &amp;&amp; element.remove();
});</pre>

<p>The fix and approach was very clean and we had authorization neatly implemented on the GUI. A possible issue here could be if the UI renders before you obtain details of user roles from backend, all the portions of html with directive will get removed from html. This did not troubled us as we were getting details of user roles as soon as login is done. But if this troubles you, a solution could be that if user roles are not yet fetch only hide the html elements and remove them only after you get list of roles. Also please note that a restricted access of GUI does not eliminates the need to a solid security implementation at backend.</p>