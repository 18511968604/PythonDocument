**IE11的Webdriver下载：**

[http://dl.pconline.com.cn/download/771640-1.html](http://dl.pconline.com.cn/download/771640-1.html)

链接：[https://pan.baidu.com/s/13TTyXGNaG5cpSNdl1k9ksQ](https://pan.baidu.com/s/13TTyXGNaG5cpSNdl1k9ksQ) 密码：2n9n

**Chrome65.0.3325.146的webdriver驱动下载：**

链接：[https://pan.baidu.com/s/1gv-ATOv\_XdaUEThQd5-QtA](https://pan.baidu.com/s/1gv-ATOv_XdaUEThQd5-QtA) 密码：dzh2

多版本：[http://chromedriver.storage.googleapis.com/index.html](http://chromedriver.storage.googleapis.com/index.html)

**Firefox58的webdriver驱动下载**

链接：[https://pan.baidu.com/s/1RATs8y-9Vige0IxcKdn83w](https://pan.baidu.com/s/1RATs8y-9Vige0IxcKdn83w) 密码：l41g





* `clear`\(\)



 Clears the text if it’s a text entry element.

* `click`\(\)



Clicks the element.

`find_element`

\(

_by='id'_

,

_value=None_

\)



‘Private’ method used by the find\_element\_by\_\* methods.

| Usage: | Use the corresponding find\_element\_by\_\* instead of this. |
| :--- | :--- |
| Return type: | [WebElement](http://selenium-python.readthedocs.io/api.html#selenium.webdriver.remote.webelement.WebElement) |

`find_element_by_class_name`

\(

_name_

\)



Finds element within this element’s children by class name.

| Args: | name: The class name of the element to find. |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_class\_name\(‘foo’\) |

`find_element_by_css_selector`

\(

_css\_selector_

\)



Finds element within this element’s children by CSS selector.

| Args: | css\_selector - CSS selector string, ex: ‘a.nav\#home’ |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_css\_selector\(‘\#foo’\) |

`find_element_by_id`

\(

_id\__

\)



Finds element within this element’s children by ID.

| Args: | id\_ - ID of child element to locate. |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | foo\_element = element.find\_element\_by\_id\(‘foo’\) |

`find_element_by_link_text`

\(

_link\_text_

\)



Finds element within this element’s children by visible link text.

| Args: | link\_text - Link text string to search for. |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_link\_text\(‘Sign In’\) |

`find_element_by_name`

\(

_name_

\)



Finds element within this element’s children by name.

| Args: | name - name property of the element to find. |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_name\(‘foo’\) |

`find_element_by_partial_link_text`

\(

_link\_text_

\)



Finds element within this element’s children by partially visible link text.

| Args: | link\_text: The text of the element to partially match on. |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_partial\_link\_text\(‘Sign’\) |

`find_element_by_tag_name`

\(

_name_

\)



Finds element within this element’s children by tag name.

| Args: | name - name of html tag \(eg: h1, a, span\) |
| :--- | :--- |
| Returns: | WebElement - the element if it was found |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_tag\_name\(‘h1’\) |

`find_element_by_xpath`

\(

_xpath_

\)



Finds element by xpath.

| Args: | xpath - xpath of element to locate. “//input\[@class=’myelement’\]” |
| :--- | :--- |


Note: The base path will be relative to this element’s location.

This will select the first link under this element.

```
myelement
.
find_element_by_xpath
(
".//a"
)
```

However, this will select the first link on the page.

```
myelement
.
find_element_by_xpath
(
"//a"
)
```

| Returns: | WebElement - the element if it was found |
| :--- | :--- |
| Raises: | NoSuchElementException - if the element wasn’t found |
| Usage: | element = element.find\_element\_by\_xpath\(‘//div/td\[1\]’\) |

`find_elements`

\(

_by='id'_

,

_value=None_

\)



‘Private’ method used by the find\_elements\_by\_\* methods.

| Usage: | Use the corresponding find\_elements\_by\_\* instead of this. |
| :--- | :--- |
| Return type: | list of WebElement |

`find_elements_by_class_name`

\(

_name_

\)



Finds a list of elements within this element’s children by class name.

| Args: | name: The class name of the elements to find. |
| :--- | :--- |
| Returns: | list of WebElement - a list with elements if any was found. An empty list if not |
| Usage: | elements = element.find\_elements\_by\_class\_name\(‘foo’\) |

`find_elements_by_css_selector`

\(

_css\_selector_

\)



Finds a list of elements within this element’s children by CSS selector.

| Args: | css\_selector - CSS selector string, ex: ‘a.nav\#home’ |
| :--- | :--- |
| Returns: | list of WebElement - a list with elements if any was found. An empty list if not |
| Usage: | elements = element.find\_elements\_by\_css\_selector\(‘.foo’\) |

`find_elements_by_id`

\(

_id\__

\)



Finds a list of elements within this element’s children by ID. Will return a list of webelements if found, or an empty list if not.

| Args: | id\_ - Id of child element to find. |
| :--- | :--- |
| Returns: | list of WebElement - a list with elements if any was found. An empty list if not |
| Usage: | elements = element.find\_elements\_by\_id\(‘foo’\) |

`find_elements_by_link_text`

\(

_link\_text_

\)



Finds a list of elements within this element’s children by visible link text.

| Args: | link\_text - Link text string to search for. |
| :--- | :--- |
| Returns: | list of webelement - a list with elements if any was found. an empty list if not |
| Usage: | elements = element.find\_elements\_by\_link\_text\(‘Sign In’\) |

`find_elements_by_name`

\(

_name_

\)



Finds a list of elements within this element’s children by name.

| Args: | name - name property to search for. |
| :--- | :--- |
| Returns: | list of webelement - a list with elements if any was found. an empty list if not |
| Usage: | elements = element.find\_elements\_by\_name\(‘foo’\) |

`find_elements_by_partial_link_text`

\(

_link\_text_

\)



Finds a list of elements within this element’s children by link text.

| Args: | link\_text: The text of the element to partial match on. |
| :--- | :--- |
| Returns: | list of webelement - a list with elements if any was found. an empty list if not |
| Usage: | elements = element.find\_elements\_by\_partial\_link\_text\(‘Sign’\) |

`find_elements_by_tag_name`

\(

_name_

\)



Finds a list of elements within this element’s children by tag name.

| Args: | name - name of html tag \(eg: h1, a, span\) |
| :--- | :--- |
| Returns: | list of WebElement - a list with elements if any was found. An empty list if not |
| Usage: | elements = element.find\_elements\_by\_tag\_name\(‘h1’\) |

`find_elements_by_xpath`

\(

_xpath_

\)



Finds elements within the element by xpath.

| Args: | xpath - xpath locator string. |
| :--- | :--- |


Note: The base path will be relative to this element’s location.

This will select all links under this element.

```
myelement
.
find_elements_by_xpath
(
".//a"
)
```

However, this will select all links in the page itself.

```
myelement
.
find_elements_by_xpath
(
"//a"
)
```

| Returns: | list of WebElement - a list with elements if any was found. An empty list if not |
| :--- | :--- |
| Usage: | elements = element.find\_elements\_by\_xpath\(“//div\[contains\(@class, ‘foo’\)\]”\) |

`get_attribute`

\(

_name_

\)



Gets the given attribute or property of the element.

This method will first try to return the value of a property with the given name. If a property with that name doesn’t exist, it returns the value of the attribute with the same name. If there’s no attribute with that name,`None`is returned.

Values which are considered truthy, that is equals “true” or “false”, are returned as booleans. All other non-`None`values are returned as strings. For attributes or properties which do not exist,`None`is returned.

| Args: | name - Name of the attribute/property to retrieve. |
| :--- | :--- |


Example:

```
# Check if the "active" CSS class is applied to an element.
is_active
=
"active"
in
target_element
.
get_attribute
(
"class"
)
```

`get_property`

\(

_name_

\)



Gets the given property of the element.

| Args: | name - Name of the property to retrieve. |
| :--- | :--- |


Example:

```
text_length
=
target_element
.
get_property
(
"text_length"
)
```

`is_displayed`

\(

\)



Whether the element is visible to a user.

`is_enabled`

\(

\)



Returns whether the element is enabled.

`is_selected`

\(

\)



Returns whether the element is selected.

Can be used to check if a checkbox or radio button is selected.

`screenshot`

\(

_filename_

\)



Saves a screenshot of the current element to a PNG image file. Returns

False if there is any IOError, else returns True. Use full paths in your filename.

| Args: | filename: The full path you wish to save your screenshot to. This should end with a.pngextension. |
| :--- | :--- |
| Usage: | element.screenshot\(‘/Screenshots/foo.png’\) |

`send_keys`

\(

_\*value_

\)



Simulates typing into the element.

| Args: | value - A string for typing, or setting form fields. For setting file inputs, this could be a local file path. |
| :--- | :--- |


Use this to send simple key events or to fill out form fields:

```
form_textfield
=
driver
.
find_element_by_name
(
'username'
)
form_textfield
.
send_keys
(
"admin"
)
```

This can also be used to set file inputs.

```
file_input
=
driver
.
find_element_by_name
(
'profilePic'
)
file_input
.
send_keys
(
"path/to/profilepic.gif"
)
# Generally it's better to wrap the file path in one of the methods
# in os.path to return the actual path to support cross OS testing.
# file_input.send_keys(os.path.abspath("path/to/profilepic.gif"))
```

`submit`

\(

\)



Submits a form.

`value_of_css_property`

\(

_property\_name_

\)



The value of a CSS property.

`id`



Internal ID used by selenium.

This is mainly for internal use. Simple use cases such as checking if 2 webelements refer to the same element, can be done using`==`:

```
if
element1
==
element2
:
print
(
"These 2 are equal"
)
```

`location`



The location of the element in the renderable canvas.

`location_once_scrolled_into_view`



THIS PROPERTY MAY CHANGE WITHOUT WARNING. Use this to discover where on the screen an element is so that we can click it. This method should cause the element to be scrolled into view.

Returns the top lefthand corner location on the screen, or`None`if the element is not visible.

`parent`



Internal reference to the WebDriver instance this element was found from.

`rect`



A dictionary with the size and location of the element.

`screenshot_as_base64`



Gets the screenshot of the current element as a base64 encoded string.

| Usage: | img\_b64 = element.screenshot\_as\_base64 |
| :--- | :--- |


`screenshot_as_png`



Gets the screenshot of the current element as a binary data.

| Usage: | element\_png = element.screenshot\_as\_png |
| :--- | :--- |


`size`



The size of the element.

`tag_name`



This element’s`tagName`property.

`text`



The text of the element.

