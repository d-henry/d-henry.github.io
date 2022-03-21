------------------------------------------------------------------------------------------
## Selenium Resources

A list of resources, documentation, courses, and tips for selenium test automation.

These resources are for both Java and Python.

## Documentation

[Python Docs](https://www.python.org/doc/)

[Java Docs](https://docs.oracle.com/en/java/)

[Selenium Docs](https://www.selenium.dev/documentation/)


## Recommended Reading 

For Python I recommend the [**Python crash course (2nd edition)**](https://www.amazon.com/Python-Crash-Course-2nd-Edition
/dp/1593279280/ref=sr_1_1?crid=2O2DHEM0XKEY5&keywords=python+crash+course&
qid=1647877891&sprefix=python+crash+course%2Caps%2C132&sr=8-1) Textbook.

For Java i was mostly taught in university classes, however [this](https://www.w3schools.com/java/java_intro.asp)
 Is a decent introduction.

For Selenium I recommend Tech With Tim's videos for an absolute beginner's introduction

They can be found [**here.**](https://www.youtube.com/playlist?list=PLzMcBGfZo4-n40rB1XaJ0ak1bemvlqumQ)

## Basic Selenium Operations in Python

- Click an element -- `some_button_element.click()`

- Send text to a field -- `some_field_element.send_keys("text here")`

- Select an option from a dropdown

There are several ways to achieve this, this is just one example.

First we find the Select element. in the Sorenson project It would look like this.

`select_element = self.browser.find_element(*self.SOME_SELECT)`

Where `browser` is our WebDriver and `*self.SOME_SELECT` is our locator reference.

Then we create a Select Object and assign the element to it

`select_object = Select(select_element)`

Then we can select by visible text

`select_object.select_by_visible_text("some option")`

- Verify an element (like a checkbox) is enabled.

`is_enabled = some_element.is_enabled()`

This will set is_enabled to either true or false, which we can then assert against using pytest

`assert(is_enabled)`


## Common issues and roadblocks

- ElementNotFound Exceptions

By far the most common issue you will ever see creating and maintaining Selenium Test Automation and one of the most
annoying to try and fix. This exception occurs (usually) when the [WebDriver](https://www.selenium.dev/documentation
/webdriver/_print/) attempts to access or manipulate an element that hasn't yet been loaded into the
[DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/
Introduction), or is not displayed.

There are several things you should check here.

First, is it a timing issue? Selenium has three types of [**waits**](https://www.selenium.dev/documentation/webdriver
/waits/) built in for us to use, and it's important to understand
all three.

+ [Explicit wait](https://www.selenium.dev/documentation/webdriver/waits/#explicit-wait)
  , typically used as a wait until some condition is true. e.g element to be clickable / visible.
+ [Implicit wait](https://www.selenium.dev/documentation/webdriver/waits/#implicit-wait)
  , used to wait for any element, this is more of a configuration setting then something you
 actually do in code.
+ [Fluent wait](https://www.selenium.dev/documentation/webdriver/waits/#fluentwait), useful for ignoring
   exceptions that may arise while waiting, such as ElementNotVisibleException or 
  ElementNotSelectableException

For more information on waits, please see the [Documentation](https://www.selenium.dev/documentation/webdriver
/waits/)
------------------------------------------------------------------------------------------






