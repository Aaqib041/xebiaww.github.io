---
layout: post
header-img: img/default-blog-pic.jpg
author: sraheja
description: 
post_id: 11522
created: 2012/02/15 10:18:56
created_gmt: 2012/02/15 05:18:56
comment_status: open
---

# Blackberry: Support For Multiple Screens (Fonts) 

<p>One of the major challenge with Blackberry Application development is adding support for multiple screens with varying sizes and pixel density. Your application must scale up or down as per the screen resolution (width, height and pixel density). In order to do it right, you must ensure scaling up and down is handled correctly for all
<table align="left" border="0">
<tbody>
<tr>
<td></td>
<td></td>
<td>
<ul>
    <li><span style="font-size: small;">Texts, </span></li>
    <li><span style="font-size: small;">Images and</span></li>
    <li><span style="font-size: small;">Layouts</span></li>
</ul>
</td>
</tr>
</tbody>
</table>
&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>in your application. In this article, we will focus primarily on Texts. So the problem statement for now is "<strong>How one can ensure all texts are scaled up correctly on multiple Blackberry devices?</strong>". I will share more information on Images and Layouts in future blogs.</p>
<p><strong><!--more--> </strong></p>
<p><strong>Base Resolution</strong>
Well to start with, pick a base screen resolution and develop your application for that. Generally it is easier to scale up, so one should chose base resolution as 320x240_162 where width = 320, height = 240 and pixel density = 162.</p>
<p><strong>Ui.UNITS_pt</strong>
Blackberry automatically scales fonts which are specified in points. So make sure, in your application, fonts for all texts are specified in points rather than pixels. Fonts specified in pixels may look smaller or larger depending on the pixel density of the device. However, those specified in points will appear almost similar on all devices.</p>
<p>For example you can set default font for your application by:
[sourcecode lang="java"]
Font font = FontFamily.forName(&quot;BBGlobal Sans&quot;).getFont(Styles.APPLICATION_FONT_STYLE,
        Styles.APPLICATION_FONT_SIZE, Ui.UNITS_pt);
FontManager.getInstance().setApplicationFont(font);
[/sourcecode]</p>
<p>where Styles is a custom class to consolidate all styles at one place. Similarily, you can change font of a RichTextField by:
[sourcecode lang="java"]
titleRTF.setFont(Font.getDefault().derive(Styles.TITLE_FONT_STYLE, 
        Styles.TITLE_FONT_SIZE, Ui.UNITS_pt));
[/sourcecode]
<strong>Run-time Validation</strong>
Automatic scaling of fonts (specified in points) is good and will resolve most of the issues related to text scaling. However, still there might be some areas where scaling has not happened as desired. For example, on a specific device, there can be a button whose label is now getting truncated or at other place text has become larger then its specified container and does not fit in any more etc etc.</p>
<p>For such boundry cases, you can make a runtime check and adjust the font size accordingly, ofcourse, in agreement with UI Designer and the Product Owner. You can do that by:
[sourcecode lang="java"]
ButtonField customButton = new ButtonField(&quot;Continue&quot;) {
        public int getPreferredWidth() {
                return Display.getWidth() / 4;
        }
};</p>
<p>Font customFont = Font.getDefault().derive(Styles.BUTTON_FONT_STYLE, 
        Styles.BUTTON_FONT_SIZE, Ui.UNITS_pt);</p>
<p>LabelField validatorLF = new LabelField(customButton.getLabel());
validatorLF.setFont(customFont);</p>
<p>int contentWidth = validatorLF.getPreferredWidth();
int i = 1;
while(contentWidth &gt; (customButton.getPreferredWidth() - Styles.CONTENT_PADDING_LEFT -
        Styles.CONTENT_PADDING_RIGHT)) {
                customFont = Font.getDefault().derive(Styles.BUTTON_FONT_STYLE, 
                                Math.max((Styles.BUTTON_FONT_STYLE - i), 3), Ui.UNITS_pt);
                validatorLF.setFont(customFont);
                contentWidth = validatorLF.getPreferredWidth();
                i++;
}</p>
<p>customButton.setFont(customFont);
[/sourcecode]</p>
<p>In the above example, we have a "Continue" button whose width is one-fourth of the screen width. For most of the devices "Continue" label scales correctly. However, on few devices, the label gets truncated.</p>
<p>So, what we are doing here is we are reducing the font size incrementally by 1 point until it fits perfectly well in the specified button size. We have also set the minimum font size to make sure it is readable. Kindly note, we have reduced the font size since we have fix container (Button) width (one-fourth of the screen size).</p>
<p>This trick comes quite handy when we have to adjust font by one or two points for few devices. In the follow-up blog, I will share my thoughts how we should handle challenges with images and layout scaling for multiple screens.</p>

## Comments

**[Jyoti Thakur](#7567 "2012-02-15 10:54:09"):** Good Information, eager to read your next blog about images and layout.

**[Sourabh Raheja](#7881 "2012-03-08 22:16:13"):** Thanks Jyoti and Omarie, would start working on the other one.

**[Omarie](#7871 "2012-03-07 15:47:23"):** Really nice post, waiting for image and layout

**[Fonts.com](#9031 "2012-06-13 08:22:38"):** WONDERFUL Post.thanks for share.you have a great blog here at http://xebee.xebia.in ! more wait ..

