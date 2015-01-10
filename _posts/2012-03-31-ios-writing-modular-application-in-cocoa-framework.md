---
layout: post
header-img: img/default-blog-pic.jpg
author: sohil
description: 
post_id: 13016
created: 2012/03/31 17:15:04
created_gmt: 2012/03/31 12:15:04
comment_status: open
---

# iOS : Writing modular application in Cocoa framework

Apple provides a a very good architecture to write good application. It in a way restricts to you to use the best practices to write cleaner code. While using the Cocoa framework for developing the mobile application you have to strictly follow a MVC pattern to design the views of your application. Following image describes the roles as well as the way communication between them happens.  ![][1]

Lets take a example to understand how it works. Lets have a view which shows a list of items in a table.To achieve this I m going to go on the story board and drag a simple table view on the default view that i already have on storyboard. I have a simple singleton model which has the list of data in my model object accessible by a list parameter.To make sure the controller for the view handles the events we need to add the following delegates and implement the following methods.

``` 


Controller Interface

@interface ViewController : UIViewController&lt;UITableViewDataSource, UITableViewDelegate&gt;

* * *

Controller Implementation

  * (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView { return 0; }

  * (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section { return [model.list count]; }

  * (UITableViewCell _)tableView:(UITableView _)tableView cellForRowAtIndexPath:(NSIndexPath _)indexPath { static NSString _CellIdentifier = @&quot;Cell&quot;; UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier]; if (cell == nil) { cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:CellIdentifier] autorelease]; }

cell.textLabel.text = [model.list objectAtIndex:indexPath.row];

return cell; }


 ```

This all perfectly works well when you have a single table in one view. While you start working on iPad we have bigger screen resolution. We start having complex view. Lets take a simple example.I have a view which has two tables and one date picker a popup window. Following are the changes that will happen in the above code.

``` 
 Controller Interface

## @interface CheckpointController : UIViewController&lt;CheckpointProtocol, UITableViewDataSource, UITableViewDelegate,UIPopOverControllerDelegate,PopOverDatePickerDelegate&gt;

Controller Implementation

The controller will have method implementations of

  * (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
  * (UITableViewCell _)tableView:(UITableView _)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
  * (void)dateSelected:(id)aValue
  * (void)popoverControllerShouldDismissPopOver: -(void)popoverControllerDidDismissPopover:


 ```

This makes your class huge and unmanageable. This is not it, since we have a two tables in same view the code complexity increases as we have to check every time which table view is calling the delegate. e.g. of numberOfRowsInSection will be

``` 


  * (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section { if(tableView == self.firstTableView){ return [firstModel.list count]; }else{ return [secondModel.list count]; } }


 ```

In this similar fashion we need to update the rest of table function.This makes a code a lot messy. And with time a big overhead to manage. Further if you have add more components to the view you will have to keep implementing more delegates.

The good solution to this problem will be to create small individual controller doing only what they are expected to and create one MainViewController which on initialization will create the controllers and extract the views from the individual controllers and add it as its subviews in the Main View. The responsibility of the main view will only be to create the controllers and handle the communication between the two individual controller if required. So lets have a look at how your MainViewController class will look like.

``` 
 MainViewController Implementation

-(void)addFirstTable{ firstTableController = [mainStoryBoard instantiateViewControllerWithIdentifier:@"FirstTable"]; UIView_ view = [(UIViewController _)firstTableController view]; view.frame = CGRectMake(0, 94, 300, 554); [self.view addSubview:[(UIViewController *)firstTableController view]]; }

-(void)addSecondTable{ secondTableController = [mainStoryBoard instantiateViewControllerWithIdentifier:@"SecondTable"]; UIView_ view = [(UIViewController _)secondTableController view]; view.frame = CGRectMake(0, 300, 200, 554); [self.view addSubview:[(UIViewController *)secondTableController view]]; }

-(void)addDatePickerView{ secondTableController = [mainStoryBoard instantiateViewControllerWithIdentifier:@"DatePicker"]; UIView_ view = [(UIViewController _)secondTableController view]; view.frame = CGRectMake(0, 500, 100, 554); [self.view addSubview:[(UIViewController *)secondTableController view]]; }

-(void)createAndAddSubview{ [self addFirstTable]; [self addSecondTable]; [self addDatePickerView]   
}


 ```

We have to call the function createAndAddSubview in view load or override the init method. So the above approach still sticks to MVC as suggested by design standards and also injects a lot of modularity into the code. Further divides the code to more classes to makes it easier to maintain.

   [1]: http://xebee.xebia.in/wp-content/uploads/2012/03/Screen-Shot-2012-03-31-at-3.58.26-PM-300x95.png (Screen Shot 2012-03-31 at 3.58.26 PM)