---
layout: post
title: Easily Detect Changes to All Text Boxes in Windows Form [C#]
category: Development
tags: [C#, computer science, programming]
---

There are a number of instances in which we might want to detect when there have been changes to text boxes within a C# Windows Forms app. As an example, I'm working on building a quick application for managing a database of natural sandstone arches within the state of Kentucky. My (very rough-draft) app has the following screen to add an arch to the database:

![_config.yml]({{ site.baseurl }}/images/winforms_text_changes/add_arch.png)

I want to have a way to detect if there have been changes within any of the text boxes when I hit the cancel button. This way, I can display a confirmation dialog to the user. 

One way I could go about this is by individually setting the *TextChanged* callback for each and every textbox. This would certainly work, but it would lead to a lot of code duplication and things would get messy quite quickly. A better way would be to programmatically set the callback for each text box. This is actually really easy to do!

# Step 1: Override the Form's **OnLoad** Method

The first thing we need to do is override our form's **OnLoad** method. This can be done quite simply:

```c#
override protected void OnLoad(EventArgs e)
{

}
```
# Step 2: Programmatically Set **TextChanged** Callback for All Text Boxes

The next step is to set the **TextChanged** callback for all our text boxes. We can do this by adding the following to our **OnLoad** method:

```c#
override protected void OnLoad(EventArgs e)
{
	foreach (Control control in this.Controls)
	{
		if (control.GetType() == typeof(TextBox))
		{
			(control as TextBox).TextChanged += Form_TextChanged;
		}
	}
}
```

All we are doing here is iterating over all the controls in our form. If the control is of the type *TextBox*, then we set the **TextChanged** callback to be the ***Form_TextChanged*** callback.

# Step 3: Build the Callback Function

Now that we have specified a callback for the **TextChanged** event, we need to actually build the callback function. In this case I'm going to keep things simple and simply set the value of a boolean property to true if a text change has been detected:

```c#
private void Form_TextChanged(object sender, EventArgs e)
{
	this.hasUnsavedChanges = true;
}
```

Now I can use this boolean property anywhere I want to test for unsaved changes (in my case, when the cancel button is clicked).
