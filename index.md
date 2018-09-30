# Table of Contents

* TOC
{:toc}

<a name="what_is_dance"></a>
# What is Dance?
Pure Java web development.

Dance is an open source, very simple, yet powerful, component based architecture for developing web applicatoins in pure Java.

As HTML, CSS and JavaScript development becomes increasingly complex, the idea is to boost productivity by creating a consistent API, hide complexity, and at the same time bring all of Java's features to front-end programming. 

<a name="features"></a>
# Features
This is in comparison to classic HTML/CSS/JavaScript development.
* Obviously, no cumbersome and time consuming HTML and CSS handcrafting.
* No messy mix of markup and programming code.
* Seamless integration between front-end and back-end.
* Less room for human errors when HTML, CSS and JavaScript are automatically generated.
* Component based development.
* Easily extensible - if needed, making custom components is very straightforward.
* Readable and easy to understand front-end code.
* Less complex and much smaller code base.
* Much easier maintenance.
* Consistent code documentation.
* Ideally, no need for testing in different browsers.
* Far better options when developing reusable code.
* Possibility to use encapsulation to hide complexity.
* More rapid prototyping and mockups.
* Common layouts that should be straightforward and intuitive to set up, now is.
* Much better error handling.
* Much less hard to find bugs and peculiarities.
* Easy to pick up front-end development by any Java back-end developer.
* Ideally, the web development team can focus on a single programming language.
* One language means better communication between front-end and back-end programmers.
* Possibility to use all features in advanced modern Java IDEs.
* Inherently simple architecture that anyone with just some Java skills will understand.
* No need to load huge CSS or JavaScript libraries or plugins. 
* Generated code is automatically minimized.
* And counting ...

<a name="disclaimer"></a>
# Disclaimer
The author takes no responsibility for use of this software. Use this software at your own risk.

<a name="requirements"></a>
# Requirements
A Java EE servlet 3.0 container server, like Tomcat, Jetty, GlassFish, JBoss, WebLogic, etc.

<a name="installation"></a>
# Installation
Just download the SparkneyDance04.jar file and add it in your class path. TODO:Link

<a name="getting_started"></a>
# Getting started
All you need to get started is few lines of code. The Hello World example is two classes, the controller and an action. The controller mapps a request to the action. For this example, controller and actions need to be in the same package.

#### Controller.java

```java
import com.sparkney.dance.core.AbstractController;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns = {"/danceguide/*"})
public class Controller extends AbstractController{

    public static Controller instance;
    
    @Override
    public void init(){
        super.init();
        instance=this;
    }
}

```

#### HelloWorldAction.java

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class HelloWorldAction extends AbstractAction{
    
    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }

    @Override
    public Component perform(Context context) throws Exception{
        return new Text("Hello world!");
    }
}
```
<a href="hello_world_action.html" target="_blank">See the result</a>

Fire up your favorite servlet container and enter this URL in the web browser

```
http://myhost/danceguide/helloWorldAction
```

where *myhost* may be something like localhost:8080 if you run locally, or your domain name if you run on a remote server.

That's it. From here, we just expand the the concept. To make a link, we add a new action with few more lines.

#### LinkAction.java

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class LinkAction extends AbstractAction{

    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }
    
    @Override
    public Component perform(Context context) throws Exception{

        LinkPanel linkPanel = new LinkPanel();
        linkPanel.setContent(new Text("Click to show Hello world"));
        linkPanel.setOnClick(new HelloWorldAction());

        return linkPanel;
    }
}
```

<a href="link_action.html" target="_blank">See the result</a>

```
http://myhost/danceguide/linkAction
```

LinkPanel is used to create links, and we link to the HelloWorldAction from previous exemple. Clickning the link will execute the HelloWorldAction.

What about adding components together? This is where layout manages come in handy. They are used to compose content and manage layout properties. Content can be any component, a text, link, or other layout managers. By combining layout manages, complex layouts can easily be achieved.

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class LayoutAction extends AbstractAction{

    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }
    
    @Override
    public Component perform(Context context) throws Exception{
        
        HorizontalLayout hLayout = new HorizontalLayout();
        hLayout.setGap(10);
        hLayout.add(new Text("Left"));
        hLayout.add(new Text("Right"));
        
        VerticalLayout vLayout = new VerticalLayout();
        vLayout.setGap(10);
        vLayout.setPadding(10);
        vLayout.setBorder(new Border());
        vLayout.add(new Text("Top")).setHAlign(Align.CENTER);
        vLayout.add(hLayout);
        vLayout.add(new Text("Bottom")).setHAlign(Align.CENTER);

        return vLayout;
    }
}
```
<a href="layout_action.html" target="_blank">See the result</a>

```
http://myhost/danceguide/layoutAction
```

<a name="elaborate_example"></a>
## Elaborate example

We don't really want any front-end code in the actions. That's where the business logic should be. Instead, we create a page component and an action that shows the page. The page is a little more complex, so we call is ComplexPage, and the action ShowComplexPage. No changes to the Controller.

#### ComplexPage.java

In this example, we are creating a new component, a responsive three colums page.

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class ComplexPage extends Component{
    
    @Override
    public void render(Context context) throws Exception{
        final Media MOBILE = new Media(0,480);
        
        Text menu = new Text("This might be a menu");
        Text content = new Text("This is some content");
        Text footer = new Text("Here goes the footer");

        Image image = new Image("http://danceguide.sparkney.com/256px-Two_dancers.jpg");
        image.setRelativeWidth(100);
        image.setTooltip("Dance image");
        
        BasicLayout layout = new BasicLayout(BasicLayout.Method.HORIZONTAL);
        layout.setMethod(MOBILE, BasicLayout.Method.VERTICAL);
        layout.setRelativeWidth(100);
        layout.setMaxWidth(800);
        layout.setPadding(10);
        layout.setCellPadding(10);
        layout.setGap(10);
        layout.setGap(MOBILE,20);
        layout.setCellBorder(new Border());
        layout.add(content).setRelativeWidth(33).setRelativeWidth(MOBILE, 100);
        layout.add(image).setRelativeWidth(33).setRelativeWidth(MOBILE, 100);
        layout.add(menu).setRelativeWidth(33).setDisplay(MOBILE, false);
        
        BasicLayout centerLayout = new BasicLayout(BasicLayout.Method.VERTICAL);
        centerLayout.setRelativeWidth(100);
        centerLayout.add(menu).setHAlign(Align.CENTER).setPadding(20).setDisplay(false).setDisplay(MOBILE, true);
        centerLayout.add(layout).setHAlign(Align.CENTER);
        centerLayout.add(footer).setHAlign(Align.CENTER).setPadding(40,0,40,0).setBackgroundColor(new Color("lightgray"));

        WindowPanel windowPanel = new WindowPanel();
        windowPanel.setContent(centerLayout);
        windowPanel.addMeta("viewport","width=device-width,initial-scale=1");
        windowPanel.render(context);
    }
}
```

#### ViewComplexPage.java

Since actions do stuff, it's a good idea to begin the name with a descriptive verb.

```java
import com.sparkney.dance.core.*;

public class ViewComplexPage extends AbstractAction{
    
    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }

    @Override
    public Component perform(Context context) throws Exception{
        return new ComplexPage();
    }
}
```
Execute tha ViewComplexPage action and checkout the result.

```
http://myhost/danceguide/viewComplexPage
```

 Change browser width and see what happens.










