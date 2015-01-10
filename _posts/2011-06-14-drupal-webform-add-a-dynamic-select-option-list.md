---
layout: post
header-img: img/default-blog-pic.jpg
author: anubhava
description: 
post_id: 9035
created: 2011/06/14 15:20:01
created_gmt: 2011/06/14 10:20:01
comment_status: open
---

# Drupal Webform : Add a dynamic select option list

Ever wanted to have a dynamic option list in your Webform select widget ( checkbox, radiobutton, dropdown ) ?

Well, I wanted to load a dynamic list of dates in a Webform and display them as checkboxes. I faced this problem when I was developing a training website where each training class would have different training dates.

_So let's start and build our dynamic select option list._

In order to achieve this you need to do the following steps:

  1. Add a select widget to your Webform 

![][1]

  2. Create a custom Webform module ( webform_custom ) with the following files.
  * **webform_custom.info** – as the name suggest contains the info of the module
[sourcecode language="php"] name = Webform Custom description = Provides custom functionality for the Webform core = 6.x project = "webform_custom" package = Other [/sourcecode] 
  * **webform_custom.module** – this file will contain the code that will generate our dynamic options list, for now just keep this file empty
  1. Active you custom module from _**Administer → Site building → Modules**_

  2. The following hook adds another item for “Load a pre-built option list” menu in Webform's select form component. This hook will go into the module file:

[sourcecode language="php"] function webform_custom_webform_select_options_info()

{
    
    
     $items = array();
    
     if (function_exists('_webform_get_dates'))
     {
    
          $items['training_dates'] = array(
    
       'title' =&gt; t('Training class dates'),
    
        'options callback' =&gt; '_webform_get_dates',
    
        );
    

}

return $items;

} [/sourcecode]

The result of the above function will add the following item in the select form component drop-down: 

![][2]

As you have noted already we still do not have the data yet. No worries!!

The following code fetches the data from the database and returns a list of all dates for a particular node/training class but you can return whatever you want as long as its an array:

[sourcecode language="php"] function _webform_get_dates()

{

$dates = array();

$node_id = arg(1);

$select = db_query(db_rewrite_sql("
    
    
              SELECT t.nid, t.field_training_date_value as tdate
    
              FROM {content_type_training_class} c, {node} n
    
              WHERE c.nid='$node_id'
    
              and c.nid=n.nid
    
              ORDER BY field_training_date_value&quot;));
    
    while ($date = db_fetch_object($select)) {
    
     $td = $date-&gt;tdate;
    
     $dates[$td] = $td;
    

}

return $dates;

} [/sourcecode]

  1. Now go back to the Webform select form component and choose the 'Training class dates' option from the “Load a pre-built option list” dropdown and save the field.

You can do a lot of customization with these hooks, which may not be possible to do via Drupal's admin interface. See the full list of Webform hooks [here][3].

That's it folks!!

   [1]: http://xebee.xebia.in/wp-content/uploads/2011/06/webform_from1-1024x457.png (webform_from)
   [2]: http://xebee.xebia.in/wp-content/uploads/2011/06/prebuilt-option-list1.png (prebuilt-option-list)
   [3]: http://api.lullabot.com/group/webform_hooks/7 (here)

## Comments

**[Pedro](#6093 "2011-11-02 15:51:35"):** Which bits of code go in which files?

**[Anubhav](#7839 "2012-03-02 22:28:59"):** @Adilson, yes the module is 'Reference'. I used this module recently in Drupal 7. I created a view and made it a 'reference' type. Then added a reference field in my content-type and assigned this View to this field.

**[Adilson](#7838 "2012-03-02 22:12:53"):** I think you could make a module to reference views to provide options to webform's list widgets. Theres a module similar, but actually it is on alpha version for drupal 6 and it provides a new widget, not pre-built options for list widget.

**[ykyuen](#7938 "2012-03-16 13:24:01"):** great post. thx a lot.

