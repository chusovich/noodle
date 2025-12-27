## Page Structure

```html
<!DOCTYPE html>
<html>

<head> <!-- all elements in the head tag are not visible on the web page -->
  <style></style>
  <title>Page Title</title> <!-- this is the title of the tab in the browser -->
  <base>example.com<base/> <!-- this is the base url used for all relative urls throughout the doc -->
  <meta> <!-- many different kinds of attributes can be added -->
  <link> <!-- attributes to link an external file, like a css sheet -->
<head/>

<body>
  <noscript> <noscript/> <!--defined html that is used when scripting has been turned off-->
  <script> <!--this is where the script functions go--> <script/>
<body/>
```

## Basic Tags

### Headings

There are six levels of headings, h1 to h6

```html
<h1>This is a heading</h1>
```

### Paragraphs

```html
<p>this is a paragraph</p>
<!-- paragraph tags also uniquely support the align attribute, value can be left, right, or center-->
<p align=center>this is a centered paragraph</p>
```

### Line Break

```html
<br> <!-- created a line break -->
```

### Horizontal Line

```html 
<hr> <!-- inserts a horizontal line -->
```

### Pre Tag

```html 
<pre>
This     paragraph has multiple
     spaces. But     it is displayed 
as it is    unlike the paragraph 
   tag.
</pre>
```

### Hyperlinks

The links destination is in the href attribute

```html
<a href="https://google.com"
   target="_top">This is the text shown for the link<a/>
```

| Target | Description |
|---|---|
| `_blank` | opens the url in a new tab |
| `_self` | opens the url in the same tab as the link (default) |
| `_parent` | opens the url in the parent frame |
| `_top` | opens the url in the full body of the window |
| `framename` | open the url in the frame specified by the name attribute

### Images

attributes

| Target | Description |
|---|---|
| `src` | either the relative or absolute url to the image |
| `alt` | shows when the image cannot be displayed |
| `width` | image width |
| `height` | image height |
| `title` | give the image a title |
| `align` | aligns the image on the page |

```html
<img src="w3schools.jpg" alt="W3Schools.com" width="104" height="142">

<h3>GeekforGeeks</h3>
<p>Adding image as link</p>
<a href="https://ide.geeksforgeeks.org/tryit.php">
	<img src= "https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190710102234/download3.png"
	alt="GeeksforGeeks logo" />
<a/>
```

### Favicons

The following code can be used in the `<head>` section

```html
<link rel="icon" type="image/png" href="path-to-favicon">
```

|Name|Size|Description|
|---|---|---|
|favicon-32.png|32×32|Standard for most desktop browsers.|
|favicon-57.png|57×57|Standard iOS home screen.|
|favicon-76.png|76×76|iPad home screen icon.|
|favicon-96.png|96×96|GoogleTV icon.|
|favicon-120.png|120×120|iPhone retina touch icon.|
|favicon-128.png|128×128|Chrome Web Store icon & Small Windows 8 Star Screen Icon*.|
|favicon-144.png|144×144|Internet Explorer 10 Metro tile for pinned site*|
|favicon-152.png|152×152|iPad touch icon.|
|favicon-167.png|167×167|iPad Retina touch icon (change for iOS 10: up from 152×152, not in action. iOS 10 will use 152×152)|
|favicon-180.png|180×180|iPhone 6 plus|
|favicon-192.png|192×192|Google Developer Web App Manifest Recommendation|
|favicon-195.png|195×195|Opera Speed Dial icon<br><br>(Not working in Opera 15 and later)|
|favicon-196.png|196×196|Chrome for Android home screen icon|
|favicon-228.png|228×228|Opera Coast icon|e

## Basic Syntax
### Elements

everything from the start tag to the end tag

elements can be nested, i.e. element can contain other elements

some elements are empty elements, meaning that they do not have closing tags

### Attributes
- All HTML elements can have attributes
- Attributes provide additional information about elements
- Attributes are always specified in the start tag
- Attributes usually come in name/value pairs like: name="value"

#### Meta Attributes

They provide basic page information in the head section of the html doc.

| Attribute  | Description                                                                      |
| ---------- | -------------------------------------------------------------------------------- |
| charset    | Determines the character encoding for the html doc                               |
| name       | Specifies the name of the metadata attribute                                     |
| content    | Provides information associated with the specified name                          |
| http-equiv | Sets an HTTP header for the content, typically used for backward compatibility   |
| scheme     | Specifies the format used to interpret the content value, often for data formats |

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,
                   initial-scale=1,
                   maximum-scale=1">
    <meta name="description" content="A Computer Science portal for geeks. 
         It contains well written, well thought 
         and well explained computer science and 
         programming articles, quizzes and practice/competitive
         programming/company interview Questions.">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>
```

#### Global Attributes

All HTML element can have the following tags

| Attribute   | Description |
| ---------- | ----------------------------------------------------------------- |
| class | group elements and allow for styling |
|style | inline css style options |
|src | specifies the source url for resources |
| contenteditable | whether the content is the element is editable |
| role | specifies the element accessibility role (for a screen magnifier, say) |
| tabindex | sets the order of the focus when navigating with the keyboard |
| id | unique identifier to that element, used in CSS and JS |
| href | the hyperlink destination, used for navigation |
| alt | used to provide alternate text for images |
| title | tip strip text |
| lang | specifies the elements language, usually in the html element |

#### Style

used to implement a css property/value

```html
<tagname style="property:value;">
<h1 style="background-color:powderblue;">This is a heading</h1>
<p style="color:red;">This is a red paragraph.</p>
<h1 style="font-family:verdana;">This is a heading</h1>
<h1 style="text-align:center;">Centered Heading</h1>
<p style="font-size:160%;">This is a paragraph.</p>
```

examples:

- background-color
- color
- font-family

#### Lang

always inside the html tag

```html
<!DOCTYPE html>  
<html lang="en">  
<body>  
...  
</body>  
</html>
```

#### Title

gives the element a tooltip

```html
<p title="I'm a tooltip">This is a paragraph.</p>
```

### Text Formatting
- `<b>` - Bold text
- `<strong>` - Important text
- `<i>` - Italic text
- `<em>` - Emphasized text
- `<mark>` - Marked text
- `<small>` - Smaller text
- `<del>` - Deleted text
- `<ins>` - Inserted text
- `<sub>` - Subscript text
- `<sup>` - Superscript text

### Quotations
- `<abbr>` - Defines an abbreviation or acronym
- `<address>` - Defines contact information for the author/owner of a document
- `<bdo>` - Defines the text direction
- `<blockquote>` - Defines a section that is quoted from another source
- `<cite>` - Defines the title of a work
- `<q>` - Defines a short inline quotation

### Comments

Single line comment:

```html
<!-- Write your comments here -->
```

Block comment:

```html
<!-- 
I have a comment
and another
-->
```

Inline comments:

```html
<p>This <!-- great text --> is a paragraph.</p>
```

## Text Formatting
### Formatting

Logical Tags: Tags convey the importance to a search engine, but don't get reflected in the appearance of the text.

Physical Tags: Tags that change the appearance of the text on the web page.

|Tags|Description| Type |
|---|---|---|
|`<i>`|Showcases italicized text.| Physical |
|`<small>`|Renders text in a smaller font size.| Physical |
|`<ins>`|Highlights added or inserted text.| Physical |
|`<sub>`|Creates subscript text.| Physical |
|`<strong>` |Emphasizes text with importance, often in bold.| Logical |
|`<b>` |Displays text in a bold format.| Physical |
|`<mark>` |Accentuates text with a background highlight.| Physical |
|`<del>` |Strikes through text to signify deletion.| Physical |
|`<em>` |Adds emphasis to text, commonly styled as italic.| Logical |
|`<sup>` |Formats text as superscript.| Physical |

### Colors

| Name | Syntax |
| ---|---|
|Color Name | `style="color: darkgreen;"` |
| RGB | `style="color: rgb(0,255,0);"`|
| RGBA | `style="color: rgb(0,255,0,0.6);"` |
| Hex | `style="color: #FF69B4;"` |
| HSL | `style="color: hsl(45, 50%, 100%);"` |
| HSLA | `style="color: hsl(45, 50%, 100%, 0.7);"`

### Quotations

| Tag | Description |
| ---|---|
|`<abbr>`|	Defines abbreviation or acronym. |
|`<address>`|	Defines contact info for the author/owner of a document.|
|`<bdo>`	| Defines text direction, left-to-right or right-to-left.|
|`<blockquote`> |	Defines a section quoted from another source.|
|`<cite>` |	Defines the title of a work, book, article, or publication.|
|`<q>` |	Defines short inline quotation, enclosed in quotation marks.|

```html
<!DOCTYPE html>
<html>

<head>
    <title>HTML Quotations Example</title>
</head>

<body>
    <h3>GeeksforGeeks</h3>
	<!--Normal text-->
    <p>
        The quick brown fox jumps over the
        lazy dog<br />
    </p>

    <!--Inside <bdo> tag-->
    <p>
        <bdo dir="rtl">The quick brown fox jumps over
            the lazy dog</bdo>
    </p>

	<!-- Insert abbreviation meaning -->
	<p>
        Welcome to
        <abbr title="GeeksforGeeks">GfG</abbr>
    </p>

	<!-- Display address -->
    <address>
        <p>
            Address:<br />
            710-B, Advant Navis Business
            Park,<br />
            Sector-142, Noida Uttar Pradesh –
            201305
        </p>
    </address>

    <!-- Inside blockquotes -->
    <blockquote>
        <p>The quick brown fox jumps
            over the lazy dog
        </p>
    </blockquote>

    <!-- Inside quotes -->
    <q>The quick brown fox jumps over the lazy dog</q>

    <!-- Cite with title -->
    <p>
        The <cite>GeeksforGeeks</cite>
        is the best site to<br>
        search for articles and practice problems.
    </p>
</body>

</html>
```