---
layout: post
header-img: img/default-blog-pic.jpg
author: brijesh pant
description: 
post_id: 19261
created: 2014/11/26 16:45:49
created_gmt: 2014/11/26 11:45:49
comment_status: open
---

# Customizing Angular Directives

<p>While developing Angular Application we have a obvious choice of using third party directives. Most of the time these directive are suffice to handle all the use cases. But situation may occur when you feel that there is really something missing to fulfill your Specific problem. Common practice is to add your own custom directive along with the directive, you want to use in the element. But It sounds quite weired to Have two directives to solve the same problem. Hence we might have to think of some different way.</p>
<p>In this Post i will share one of the Scenerio in above said category and solution to that of-course.</p>
<p><b>Scenario:</b> </p>
<p>Let us take an example of uibootstrap datepicker-popup directive to show the datepicker. This directive can be used in HTML like this</p>
<pre>
 [input type="text" class="form-control" datepicker-popup="{{format}}" 
      ng-model="date" data-show-button-bar="false" data-datepicker-mode ="mode" is-open="opened" close-text="Close"]
</pre>

<p>It will do pretty much all the jobs one can expect from the date picker. As per the requirements you can add more options available with this directive. You might use this date picker in several places across your application. Most of the time you will find same configuration. For this you will be repeating the same piece of code again and again. I don't really like this and sure you as well.</p>
<p>There is one more scenario. Your back end API gives you date in different format than what you have to display in the GUI and similarly you have to send back to API in different format.
For this  we have to first change the date string to date before passing to the directive. And also change the date before sending back to back end API. Again you are going to do it in every controller managing the HTML.</p>
<p>To handle both the above cases we can write a custom directive that will internally use the datepicker-popup directive. We will provide all the options in custom directive and then this can be used every where else.</p>
<p>Here is the example</p>
<pre>
directive('CustomDatepicker', function ($compile, $filter) {
         return {
             restrict: 'A',
             scope   : {
                   CustomDatepicker: "="
             },
             link    : function ($scope, element) {
               $scope.mode = "year";
               var timestamp = Date.parse($scope.CustomDatepicker);
               if (!isNaN(timestamp)) {
                   $scope.date = new Date(timestamp);
               }

               element.attr('data-datepicker-popup', "dd-MM-yyyy");
               element.attr('data-ng-model', 'date');
               element.attr('data-is-open', 'datePicker.open');
               element.attr('data-show-button-bar', false);
               element.attr('data-ng-click', 'datePicker.open = true');
               element.attr('data-datepicker-mode', 'mode');
               element.removeAttr("data-custom-datepicker");
               $scope.$evalAsync(function (scope) {
                   $compile(element)(scope);
             });

             $scope.$watch('date', function () {
               if ($scope.date) {
                   $scope.customDatepicker = $filter('date')($scope.date, "yyyy-MM-dd");
               }
             });

         }
        };
      });
    });

</pre>

<p><br class="none" /></p>
<p><b>Explanation:</b> </p>
<p class="none">In the custom directive i have defined the scope variable customDatepicker that holds the date value as received by your controller. Now change this date string to proper date

<pre>
var timestamp = Date.parse($scope.customDatepicker);
if (!isNaN(timestamp)) {
$scope.date = new Date(timestamp);
}</pre>


Now we will add datepicker and all of the other options that we needed. This can be done using attr method of the element.

<pre>

element.attr('data-datepicker-popup', "dd-MM-yyyy");
element.attr('data-ng-model', 'date');
element.attr('data-is-open', 'datePicker.open');
element.attr('data-show-button-bar', false);
element.attr('data-ng-click', 'datePicker.open = true');
element.attr('data-datepicker-mode', 'mode');</pre>

Remove custom-datepicker as it is no longer needed. 

element.removeAttr("data-custom-datepicker");

to apply the DOM changes in the link function add following lines

<pre>
$scope.$evalAsync(function (scope) {
$compile(element)(scope);
});
</pre>

it will update the DOM in next digest cycle.

Finally as we have bind different scope variable with the date picker, our original scope variable remain unchanged. So we need this to be updated . To achieve this we will add watch on date And update the  customDatepicker whenever date will be changed

<pre>$scope.$watch('date', function () {
if ($scope.date) {
$scope.customDatepicker = $filter('date')($scope.date, "yyyy-MM-dd");
}
});</pre>

In the HTML we just have to add following code

<pre>
[input type="text" data-custom-datepicker="input.date"]
</pre>

This will implicitly add date-picker and all the associated options.Also you need not to worry for the date format in every controller.
 Date format that will be displayed is dd-MM-yyyy and the output format will be yyyy-MM-dd.

Similarly you can customize your  other directives as well.


Happy coding....