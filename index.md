Document hosted at https://sparkney.github.io/danceguide/

# What is Dance
Dance is an open architecture for developing web applicatoins i pure Java.

## Code example

```java
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
