---
title: Spamming Fake Forms with Selenium
date: 2020-05-22
tags:
  - python
  - selenium
layout: layouts/post.njk
---

There must be at least one time that you got a spam email asking you to verify some online account that you have. The link in the email redirects you to a seemingly legit site that asks you your account details. These type of schemes are knows as **phishing** attacks. It is a common technique used by hackers and scammers to extract bank details and sensitive information. Sadly, countless people fall for this daily.

Recently I got a similar email asking me to verify my email account and I thought that I should do something about it. So, I made a python script that would spam the form with fake email accounts and passwords. This way it would be hard for them to separate the fake credentials from the real ones.

## Selenium web driver

For this I used a library called [Selenium](https://www.selenium.dev/). It is used to control your web browser by without real human interaction. Websites think that it is an actual person who is using the website but in reality it is your computer controlling everything.

Most websites don't have the functionality to differentiate a machine from an actual person so this will come in handy. I know that there are much easier ways to do this other than using Selenium, but I wanted to try out Selenium for a change.

For Selenium to work you need a web driver depending on your browser. For chrome, the driver can be found [here](https://chromedriver.chromium.org/downloads). You can google search for other drivers.

Then all you have to do is

```python
from selenium import webdriver
driver_location = "./chromedriver"
driver = webdriver.Chrome(driver_location)
```

Here `driver_location` is the path to the downloaded driver, in this case it was the same directory as my python script.

Next, you have to load the site in the browser.

```python
driver.get("https://facebook.com")
```

If you run the script now, you will see that a browser window will automatically open the website you provided in `get` function.

To click on web elements or filling forms, you have to first find out the exact HTML `id` or XPath of the element on the web page. For that you have to open the developer console using `F12` (chrome) depending on your browser and inspect the form input element. When you find out the element in the source, right click the element and copy the XPath in the `Copy` dropdown menu.

![facebook email xpath](/img/xpath-facebook-email.png "facebook email xpath")

You can also use the `id` HTML tag to identify the element instead of using the XPath.

To find the element we use `find_element` functions of Selenium.

```python
driver.find_element_by_xpath('//*[@id="email"]')
# or
driver.find_element_by_id('email')
```

Now that you have the field, you can send keyboard input using `send_keys` function.

```python
driver.find_element_by_id('email').send_keys('blah@email.com')
```

If you run the script now, you will see that the web browser opens and automagically types in the email address. We do the same process for the password field.

To click buttons, you have to use `click` instead of `send_keys`

```python
driver.find_element_by_id('login').click()
```

That's pretty much it on how Selenium works. Now, let's see how we can use this amazing tool to scam scammers.

## The Script

To fill out the form, first we need fake information. I used `names` library to get random names. I also used these names to create fake e-mail addresses.

```python
def random_person(password_length = 8):
    # name
    full_name = names.get_full_name()
    # email
    name_parts = full_name.lower().split(" ")
    email = "".join(name_parts) + "@gmail.com"
    # password
    lettersAndDigits = string.ascii_letters + string.digits
    password = ''.join((random.choice(lettersAndDigits) for i in range(password_length)))
    return [full_name, email, password]
```

To make this more believable, you can select a password from a dictionary of common passwords. Not everyone have truly random password (you should) and it would be less believable if all the passwords that we sent are random. You can also take random email domains instead of only Gmail.

Now that fake credentials are created, we just have to send these details to the website form using Selenium.

### Problems I encountered

There were a few roadblocks that I stumbled upon while writing the script. First of all, the form was not being filled because the text fields were not visible on the screen. You have to scroll down to the element and when the element was visible, only then you can fill the form.

Scrolling in Selenium is easy.

```python
# this would scroll down to the bottom of the page
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

# this would scroll to the element
from selenium.webdriver.common.action_chains import ActionChains
ActionChains(driver).move_to_element(driver.sl.find_element_by_id('login')).perform()
```

Another issue was that the site was dynamically changing the `id` of the form elements when some text was entered or an element was clicked. An easy solution to this is storing the elements first before entering any text or clicking anything.

```python
# find and store the elements
email_element = driver.find_element_by_id('email')
password_element = driver.find_element_by_id('password')
name_element = driver.find_element_by_id('fullname')

# now that Selenium already knows the form elements'
# location, no need to worry about id change
[name, email, password] = random_person()
email_element.send_keys(email)
password_element.send_keys(password)
name_element.send_keys(name)
```

There was also times when Selenium would not click the button even when it is visible on screen. The problem was that sometimes the buttons get disabled for a few milliseconds while someone is typing. Since your computer is a machine, it is quick enough to type and immediately click the button while it is disabled.

To solve this you simply put the script to sleep as soon as you are done typing by using `time` module.

```python
import time

# code to type in information ...
time.sleep(2) # sleep for 2 seconds
# code to submit the form ...
```

To do this repeatedly you just put all this in a while loop. To refresh the page or go back to unfilled form just use `driver.refresh()` or `driver.get()` in the end of the loop.

## How to spot phishing sites and e-mails

It is increasingly important to protect ourselves from such phishing attacks these days. There are a lot of people who don't know much about this and get scammed easily. Below are a few ways to spot such scams:

- Always click links in emails from trusted sources.
- Double check the URL before entering your credentials. Usually scammers redirect you to a legit looking web page and sometimes there is no visual difference at all. The only way to be sure is to look at the URL. Usually it is similar to the original website address but a little different. For example, `www.facebool.com` and `www.goo8le.com`.
- Same goes for emails. They seem to be coming from legit source but often they are not.
- Check for spelling mistakes. Most of these scams have poor grammar usage and some spelling mistakes.
- Only submit personal information if your connection is using HTTPS. See the URL of the website, if it starts with `http://` instead of `https://`, chances are that there is something shady going on.
