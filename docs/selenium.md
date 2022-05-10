------------------------------------------------------------------------------------------
## Selenium Resources

A list of resources, documentation, courses, and tips for selenium test automation.

These resources are for both Java and Python.

## Documentation and links

[Python Docs](https://www.python.org/doc/)

[Java Docs](https://docs.oracle.com/en/java/)

[Selenium Docs](https://www.selenium.dev/documentation/)

[Why POM?](https://www.guru99.com/page-object-model-pom-page-factory-in-selenium-ultimate-guide.html)

[Sample Projects](https://github.com/d-henry/Selenium-Samples) W.I.P


## Recommended Reading 

For Python I recommend the [**Python crash course (2nd edition)**](https://www.amazon.com/Python-Crash-Course-2nd-Edition
/dp/1593279280/ref=sr_1_1?crid=2O2DHEM0XKEY5&keywords=python+crash+course&
qid=1647877891&sprefix=python+crash+course%2Caps%2C132&sr=8-1) Textbook.

For Java i was mostly taught in university classes, however [this](https://www.w3schools.com/java/java_intro.asp)
 Is a decent introduction.

For Selenium I recommend Tech With Tim's videos for an absolute beginner's introduction

They can be found [**here.**](https://www.youtube.com/playlist?list=PLzMcBGfZo4-n40rB1XaJ0ak1bemvlqumQ)

## Basic Selenium Operations in Python

- [Click](https://www.selenium.dev/documentation/webdriver/elements/interactions/#click) an 
  element -- `some_button_element.click()`

- [Send text](https://www.selenium.dev/documentation/webdriver/elements/interactions/#send-keys) to a 
  field -- `some_field_element.send_keys("text here")`

- [Select](https://www.selenium.dev/documentation/webdriver/elements/select_lists/) an option from a dropdown or select list

There are several ways to achieve this, this is just one example.

First we find the Select element. in the Sorenson project It would look like this.

`select_element = self.browser.find_element(*self.SOME_SELECT)`

Where `browser` is our [WebDriver](https://www.selenium.dev/documentation
/webdriver/_print/) and `*self.SOME_SELECT` is our locator reference.

Then we create a Select Object and assign the element to it

`select_object = Select(select_element)`

Then we can select by visible text

`select_object.select_by_visible_text("some option")`

- Verify an element (like a checkbox) is enabled.

`is_enabled = some_element.is_enabled()`

This will set is_enabled to either true or false, which we can then assert against using pytest

`assert(is_enabled)`

## Working with Waits to increase test reliability

Selenium is renowned for being fragile and inconsistent. I aim to mitigate that as much as possible whenever I'm writing
tests. The first thing I do to this end is to use unique id's whenever possible, the second, is to use waits.

Here is an example of me accessing an element using a WebDriverWait in Java, which I'll break down and explain.

`//locate the element`

`WebElement settings_button = (new WebDriverWait(driver, 10))`
`.until(ExpectedConditions.elementToBeClickable(By.id("settings_id")));`

`//click the settings button`

`settings_button.click();`

Let's break this down...

`WebElement settings_button =`
Here i'm declaring a new WebElement object, named settings_button.

`(New WebDriverWait(driver,10))`
This bit is a new WebDriverWait object, into which we pass two parameters, 1. our webDriver and 2. a time to wait (in seconds)
for an element to be found. If it takes our driver longer than this time to find the element an exception will be thrown.

`.until(ExpectedConditions.elementToBeClickable`
We tell the WebDriverWait to wait until the element is "clickable" meaning loaded, displayed, visible, and interactable.

`(By.id("settings_id)));`
Here we specify our locator type (id) and our desired elements id.

`settings_button.click();`
Finally we are ready to interact with our element.




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

## Why isn't intellisense working for selenium???

This is a problem I ran into, and have yet to solve for python with selenium unless i'm using Pycharm.

If i'm using pycharm my solution was to use a [Type Hint](https://www.jetbrains.com/help/pycharm/type-hinting-in-product.html)

Otherwise it's most commonly a package / import issue. make sure you followed the correct steps to install selenium for
the language you are using and that it has been added to the project in your [IDE](https://www.codecademy.com/article/what-is-an-ide).
------------------------------------------------------------------------------------------






