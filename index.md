# What is Dance
Dance is an open architecture for developing web applicatoins i pure Java.

# Installation
Just download the SparkneyDance30.jar file and put it in your calss path.

# How to begin
The quickest way to try it out, just fire up a JSP-page and do some dancing.

```jsp
<%@page import="com.sparkney.dance.core.*"%>
<%@page import="com.sparkney.dance.gui.base.*"%>

<%

Text text = new Text("This is a dance component.");

WindowPanel windowPanel = new WindowPanel();
windowPanel.setContent(text);

ComponentContext context = new ComponentContext(request, response);
windowPanel.render(context);

%>
```


# Code examples

```java
//A simle example, just to see how i works.

Font font = new Font().setFamily("arial").setSize(16);

Text text = new Text("Some text goes here");
text.setFont(font);

Image image = new Image("http://danceguide.sparkney.com/256px-Two_dancers.jpg");
image.setRelativeWidth(100);

HorizontalLayout layout = new HorizontalLayout();
layout.setCellBorder(new Border());
layout.setCellPadding(10);
layout.setGap(10);
layout.setMaxWidth(400);
layout.add(image).setRelativeWidth(50);
layout.add(text).setRelativeWidth(50).setHAlign(Align.CENTER);

WindowPanel windowPanel = new WindowPanel();
windowPanel.setContent(layout);
windowPanel.render(context);

```
<a href="simple_example.html" target="_blank">See the result</a>


```java
//A more complex and responsive example
        
final Media MOBILE = new Media(0,480);

Text menu = new Text("This might be a menu");

Text content = new Text("This is some content");

Image image = new Image("http://danceguide.sparkney.com/256px-Two_dancers.jpg");
image.setRelativeWidth(100);
image.setTooltip("Dance image");
        
BasicLayout layout = new BasicLayout(BasicLayout.Method.HORIZONTAL);
layout.setMethod(MOBILE, BasicLayout.Method.VERTICAL);
layout.setRelativeWidth(100);
layout.setMaxWidth(800);
layout.setCellPadding(10);
layout.setGap(10);
layout.setGap(MOBILE,20);
layout.setCellBorder(new Border());
layout.add(content).setRelativeWidth(33);
layout.add(image).setRelativeWidth(33);
layout.add(menu).setRelativeWidth(33).setDisplay(MOBILE, false);
        
BasicLayout centerLayout = new BasicLayout(BasicLayout.Method.VERTICAL);
centerLayout.setRelativeWidth(100);
centerLayout.add(menu).setHAlign(Align.CENTER).setPadding(20).setDisplay(false).setDisplay(MOBILE, true);
centerLayout.add(layout).setHAlign(Align.CENTER);

WindowPanel windowPanel = new WindowPanel();
windowPanel.setContent(centerLayout);
windowPanel.render(context);
```
<a href="complex_example.html" target="_blank">See the result</a>

