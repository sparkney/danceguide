# What is Dance?
Dance is front- and back-end development in pure Java.

Dance is an open, very simple, yet powerful, component based architecture and implementation for developing web applicatoins in pure Java.

As HTML, CSS and JavaScript development becomes increasingly complex, the idé is to increase productivity by creating a consistent API, hide complexity, and at the same time bring all of Java's features to front-end programming. 

# Features, in comparison to HTML/CSS/JavaScript
* Obviously, no cumbersome and time consuming HTML and CSS handcrafting.
* No messy mix of markup and programming code in the front-end.
* Seamless integration between front-end and back-end.
* Underlying HTML and CSS is automatically generated, with much less room for human errors.
* Component based development.
* Easily extensible - making custom components is very straightforward.
* Readable and easy to understand application code.
* Much smaller code base.
* Much easier maintenance.
* Makes documentation in a consistent way possible.
* Ideally, no need for testing in different browsers.
* Possibility to develop reusable code.
* Possibility to use encapsulation to hide complexity.
* More rapid prototyping and mockups.
* Common layouts that should be straightforward and intuitive to set up, now is.
* Much better error handling.
* Much less hard to find bugs and peculiarities.
* Easy to pick up front-end development by any Java developer.
* Easier to become a full-stack web developer.
* Ideally, the web development team only need one programming language, Java.
* One language means better communication between front-end and back end programmers.
* Ideally, future proof, since the API is not dependent on the underlying technologies.
* Possibility to use all features in advanced Java IDEs, like auto-completion and auto-refactoring.
* Inherently simple architecture that anyone with just some Java-skills can understand.

# Installation prerequisites
A Java EE servlet-container server, like Tomcat, Jetty, GlassFish, JBoss, WebLogic, etc.

# Installation
Just download the SparkneyDance30.jar file and put it in your calss path. TODO:Link

# Disclaimer
The author takes no responsibility for use of this software. Use this software at your own risk.

# How to begin
The quickest way to try it out, just fire up a JSP-page and do some Dancing.

```
<%@page import="com.sparkney.dance.core.*" %>
<%@page import="com.sparkney.dance.gui.base.*" %>

<%

Text text = new Text("This is a dance component.");

WindowPanel windowPanel = new WindowPanel();
windowPanel.setContent(text);

ComponentContext context = new ComponentContext(request, response);
windowPanel.render(context);

%>
```
<a href="jsp_example.html" target="_blank">See the result</a>


# Code examples

```java
//A simple example, just to illustrate how i works.

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
//A more complex and responsive example.
//Try changing browser width and see what happens.

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

