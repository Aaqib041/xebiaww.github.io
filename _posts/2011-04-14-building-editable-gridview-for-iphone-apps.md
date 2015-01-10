---
layout: post
header-img: img/default-blog-pic.jpg
author: Rajdeep
description: 
post_id: 8310
created: 2011/04/14 23:05:22
created_gmt: 2011/04/14 18:05:22
comment_status: closed
---

# Building Editable GridView for iPhone apps

<p><a rel="attachment wp-att-8376" href="http://xebee.xebia.in/2011/04/14/building-editable-gridview-for-iphone-apps/screen-shot-2011-04-13-at-11-19-34-pm/"><img class="size-medium wp-image-8376  alignright" style="margin-left: 0px; margin-right: 0px;" title="Editable Grid View-Kalorific" src="http://xebee.xebia.in/wp-content/uploads/2011/04/Screen-shot-2011-04-13-at-11.19.34-PM-232x300.png" alt="Editable Grid View-Kalorific" width="150" height="200" /></a></p>

<p>Grid Views are powerful way of presenting data to users that won't be as appealing if presented in tabular form. For instance, the default home screen of iPhone looks elegant when presented in grid view. Similar case can occur in your application as well. As for iPhone, sdk doesn't provide any grid view implementation similar to that provided by Android(Non-Editable).  So I thought of building an <strong>editable grid view</strong> as shown on right side .<br />
<!--more--><br />
Custom GridView can be build in two ways, programmatically adding components to default cell or by creating custom UITableViewCell. We will be creating custom cells containing 3 custom buttons having 3 labels respectively. This can be done easily by using interface builder. The next step is to supply this custom cell to the tableview and filling it with required data. More detailed help regarding custom cells can be found in this <a href="http://developer.apple.com/library/ios/#samplecode/AdvancedTableViewCells/Introduction/Intro.html%23//apple_ref/doc/uid/DTS40009111">sample code</a> by apple.</p>

<p>Further we will use</p>

<pre lang="javascript">self.tableView.separatorColor=[UIColor clearColor];
self.tableView.backgroundColor=[UIColor clearColor];
self.tableView.separatorStyle=UITableViewCellSeparatorStyleNone;</pre>

<p>in viewDidLoad and</p>

<pre lang="javascript">cell.backgroundColor=[UIColor clearColor];</pre>

<p>in cellForRowAtIndexPath to clear background colors associated with Table and its cells.</p>

<p><a rel="attachment wp-att-8359" href="http://xebee.xebia.in/2011/04/14/building-editable-gridview-for-iphone-apps/screen-shot-2011-04-13-at-10-49-09-pm/"><img class="size-medium wp-image-8359  alignright" title="Edit mode of editable grid view-Kalorific" src="http://xebee.xebia.in/wp-content/uploads/2011/04/Screen-shot-2011-04-13-at-10.49.09-PM-235x300.png" alt="Edit mode of editable grid view-Kalorific" width="150" height="200" /></a></p>

<p>Till now you would have been able to setup simple grid view, so now it's time to move on to editing the grid view. But before doing that, one should ask why we want an editable grid view? <strong>The answer is usability</strong>. In current implementations the user has to navigate to different view for editing items associated with grid. But he/she would always like to have all the controls specific to an item on one screen so that he/she would not have to navigate for editing(deleting/adding). Having said this we provide him/her with a default edit button on navigation bar and will build a view similar to the one given on the right side when he/she taps the edit button.</p>

<p>The following steps are needed to convert grid view to editable grid view.</p>

<ul>
<li>When user taps on edit button
<pre lang="javascript">-(void)setEditing:(BOOL)editing animated:(BOOL)animated</pre>
<p>is called. Here we delete all the previous sections present and then insert new sections to transit between grid mode and editing mode. Inserting new sections(after deleting all previous sections) invokes UITableView <strong>Data Source</strong> methods which are handled as discussed below.</li>
<li>We use
<pre lang="javascript">@property(nonatomic,getter=isEditing) BOOL editing.</pre>
<p>for specifying number of rows or sections when in editing and grid mode because in both cases the rows or sections will be different. For instance in our case in grid mode number of rows will be 3 and in editing mode number of rows will be 9.</li>
<li>We use two different UITableViewCells(Custom and Default), first for presenting grid view and second for presenting items in editable mode. We use different table <strong>cell identifiers</strong> for custom and default cells to resolve any conflicts that may occur due to reusable cells returned by dequeueReusableCellWithIdentifier.<br />
[sourcecode lang="javascript"]&lt;br /&gt;
    //in editable mode default cells are returned&lt;br /&gt;
    if (tableView.editing) {&lt;br /&gt;
        static NSString *CellIdentifier = @&amp;amp;quot;Cell&amp;amp;quot;;&lt;br /&gt;
         UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];&lt;br /&gt;
        if (cell == nil) {&lt;br /&gt;
            cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleValue1      reuseIdentifier:CellIdentifier] autorelease];&lt;br /&gt;
                   }&lt;br /&gt;
        return cell;&lt;br /&gt;
      }&lt;br /&gt;
    //in grid view custom cells are returned&lt;br /&gt;
    else&lt;br /&gt;
    {&lt;br /&gt;
    static NSString *CellIdentifier = @&amp;amp;quot;CustomCell&amp;amp;quot;;&lt;br /&gt;
    CustomTableViewCell *cell = (CustomTableViewCell *)[tableView dequeueReusableCellWithIdentifier:CellIdentifier];&lt;br /&gt;
    if (cell == nil)&lt;br /&gt;
    {&lt;br /&gt;
        [[NSBundle mainBundle] loadNibNamed:@&amp;amp;quot;CustomTableViewCell&amp;amp;quot; owner:self options:nil];&lt;br /&gt;
        cell = customCell;&lt;br /&gt;
    }&lt;br /&gt;
return cell;&lt;br /&gt;
}&lt;br /&gt;
[/sourcecode]</li>
<li>When the user taps the done button above steps are repeated but with grid view data.</li>
</ul>

<p>This type of grid view is light and easy to create, giving the developer more power to customize the grids that he wants to create. The major drawback associated with it is of responding to orientation changes because images are stretched thus distorting the look and feel of the grid. I will try to find an elegant way for handling orientation changes and upload the new code.As for now you can find the code <a href="https://github.com/xebia/xebiaindiamobile/tree/master/EditableGridView">here</a>.</p>

## Comments

**[Tahee Kim](#5710 "2011-07-12 20:06:01"):** I have a question. I don't know how to scroll up the screen after I clicked the image button. The scroll is ok when I clicked outside area of the button. When I want to scroll up the screen on clicking the button, how can I do?? Thank you for reply in advance.

**[Rajdeep Mann](#5713 "2011-07-13 18:01:38"):** It should be working fine as button click is an independent event which has nothing to do with scrolling of the view. Are you able to click other buttons after clicking a button?

