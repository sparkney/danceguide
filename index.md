# Table of Contents

* TOC
{:toc}

# What is Dance?
Pure Java web development.

Dance is an open source, very simple, yet powerful, component based architecture for developing web applicatoins in pure Java.

As HTML, CSS and JavaScript development becomes increasingly complex, the idea is to boost productivity by creating a consistent API, hide complexity, and at the same time bring all of Java's features to front-end programming. 

# Features

<span style="color:lightblue">TODO: Break up into subcategories.</span>

This is in comparison to classic HTML/CSS/JavaScript development.
* Obviously, no cumbersome and time consuming HTML and CSS handcrafting.
* No messy mix of markup and programming code.
* Layout managers handle component layout.
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

# Disclaimer
This software is distributed under the [BSD 3-Clause License](/LICENSE.html)

# Requirements
A Java EE servlet 3.0 container server, like Tomcat, Jetty, GlassFish, JBoss, WebLogic, etc.

# Installation
Just download the SparkneyDance04.jar file and add it in your class path. TODO:Link

# Getting started
All you need is few lines of code. The Hello World example is two classes, the controller and an action. The controller mapps a request to the action. For this example, controller and actions need to be in the same package.

## Hello world

*Controller.java*

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

*HelloWorldAction.java*

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

## Create a link

To make a link, we create a new action with two more lines.

*LinkAction.java*

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
        linkPanel.onClick(new HelloWorldAction());

        return linkPanel;
    }
}
```

<a href="link_action.html" target="_blank">See the result</a>

```
http://myhost/danceguide/linkAction
```

LinkPanel is used to create a link to HelloWorldAction from previous exemple. Clickning the link will execute the HelloWorldAction.

## Layout manager

To add components together, we use layout manages. They are used to compose content and manage layout properties. Content can be any component, a text, link, or other layout managers. By combining layout manages, complex layouts can easily be achieved.

*LayoutAction.java*

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

## Action parameters

This example shows how to send parameters with an action. Clicking the button will execute the action itself and print the parameter.

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class ParameterAction extends AbstractAction{

    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }
    
    @Override
    public Component perform(Context context) throws Exception{
        
        //Get the parameter value
        String paramValue = context.getRequest().getParameter("paramName");
        
        //Create an action and add a parameter
        ParameterAction action = new ParameterAction();
        action.addParameter("paramName", "You clicked the action button");
        
        //Create a button to execute the action
        Button actionButton = new Button();
        actionButton.setText("Action »");
        actionButton.onClick(action);
        
        //Layout the component
        VerticalLayout layout = new VerticalLayout();
        layout.setPadding(10);
        layout.setGap(5);
        layout.add(actionButton);
        
        //If there is a parameter value, add it to the layout
        if(paramValue!=null){
            layout.add(new Text(paramValue));
        }
        
        //We always need the window panel
        WindowPanel windowPanel = new WindowPanel();
        windowPanel.setContent(layout);

        return windowPanel;
    }
}
```

<a href="parameter_action.html" target="_blank">See the result</a>

```
http://myhost/danceguide/parameterAction
```

## A simple form

This example is similar to the previous one, and is shows how to create and submit a form.

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class FormAction extends AbstractAction{

    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }
    
    @Override
    public Component perform(Context context) throws Exception{
        
        //Get the request parameter value
        String paramValue = context.getRequest().getParameter("paramName");
                
        //Create a text field
        TextField textField = new TextField();
        textField.setName("paramName");
        textField.setValue("You submitted the form");
        
        //Create a submit button
        Button submitButton = new Button();
        submitButton.setText("Submit");
        submitButton.onClick(new SubmitForm(new FormAction()));
        
        //Layout the components
        VerticalLayout layout = new VerticalLayout();
        layout.setPadding(10);
        layout.setGap(5);
        layout.add(textField);
        layout.add(submitButton).setHAlign(Align.RIGHT);
        
        //If there is a parameter value, add it to the layout
        if(paramValue!=null){
            layout.add(new Text(paramValue));
        }

        //This is a form, don't forget the form panel
        FormPanel formPanel = new FormPanel();
        formPanel.setContent(layout);
        
        //We always need the window panel
        WindowPanel windowPanel = new WindowPanel();
        windowPanel.setContent(formPanel);

        return windowPanel;
    }
}
```

<a href="form_action.html" target="_blank">See the result</a>

```
http://myhost/danceguide/formAction
```

## Responsive layout

We don't really want any front-end code in the actions. That's where the business logic should be. Instead, we create a page component and an action that shows the page. It's a responsive three colums page. No changes to the Controller.

*ComplexPage.java*

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
        layout.setGap(10).setGap(MOBILE,20);
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

Since actions do stuff, it's a good idea to begin the name with a descriptive verb.

*ViewComplexPage.java*

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

<a href="responsive_page.html" target="_blank">See the result</a>

Execute the ViewComplexPage action and checkout the result. Try changing browser width and see what happens.


```
http://myhost/danceguide/viewComplexPage
```

## Ajax actions

An ajax actions typically returns a component that updates an existing element. An ajax action works alot like standard actions, only we need to state what target element the result should end up in. To achieve this, we give the target element an ID, and we tell the ajax actions to put the result in the element by that ID.

First we create the ajax action. It extracts a message parameter, calculates the number of characters in the message, and returns a result as a component.

*AjaxSubmit.java*

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class AjaxSubmit extends AjaxAction{
    
    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }

    @Override
    public Component perform(Context context) throws Exception{
        String message = context.getRequest().getParameter("message");

        String serverName = context.getRequest().getServerName();
        
        int lenght = message.length();
        
        Text resultText = new Text(serverName + " says message is "
                + lenght + " characters long");
                
        return resultText;
    }
}

```

We create a page with a form, from which we can submit the ajax action. Note how we set the target element ID for the ajax axtion. We also create an empty layout cell and give it the same ID. 

*AjaxPage.java*

```java
import com.sparkney.dance.core.*;
import com.sparkney.dance.gui.base.*;

public class AjaxPage extends Component{
    
    @Override
    public void render(Context context) throws Exception{
        AjaxSubmit submitAjax = new AjaxSubmit();
        submitAjax.setTargetElementId("textbox");

        TextField textField = new TextField();
        textField.setRelativeWidth(100);
        textField.setName("message");
        textField.onKeyUp(new SubmitForm(submitAjax));
        
        VerticalLayout layout = new VerticalLayout();
        layout.setPadding(10);
        layout.setGap(5);
        layout.setWidth(200);
        layout.add(new Text("Enter a message"));
        layout.add(textField);
        layout.add(new Space()).setId("textbox");
        
        FormPanel formPanel = new FormPanel();
        formPanel.setContent(layout);

        WindowPanel windowPanel = new WindowPanel();
        windowPanel.setContent(formPanel);
        windowPanel.render(context);
    }
}
```

Finally, we create a standard action for viewing our page.

```java
import com.sparkney.dance.core.*;

public class ViewAjaxPage extends AbstractAction{
    
    @Override
    public void init(){
        setActionMapper(Controller.instance.getActionMapper());
    }

    @Override
    public Component perform(Context context) throws Exception{
        return new AjaxPage();
    }
}
```

```
http://myhost/danceguide/viewAjaxPage
```

When you type some text in the text box, ajax requests are sent to the server. The server returns a component that is placed in the target element.  





