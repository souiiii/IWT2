# Module III: XML, AJAX, jQuery and JSON

These notes cover the Module III syllabus except **React.js and Node.js**, which are excluded. The emphasis is on understanding and explaining concepts, with enough syntax to support an exam answer.

**Contents:** [XML](#1-xml-structuring-and-validating-data) · [AJAX](#2-ajax-communicating-with-the-server-without-reloading-the-page) · [jQuery](#3-jquery-selecting-and-changing-web-page-elements) · [JSON](#4-json-representing-and-exchanging-data) · [Revision check](#5-final-revision-check)

The four technologies play different roles. **XML and JSON describe data. AJAX exchanges data with a server. jQuery is a JavaScript library that simplifies tasks such as selecting and changing page elements.** They can work together, but none is a replacement for all the others.

# 1. XML: Structuring and validating data

## 1.1 What XML is and why it is used

**XML (Extensible Markup Language)** represents structured data using tags chosen by the document's author. For example, `<price>450</price>` identifies a value as a price. Unlike a programming language, XML does not execute instructions; software must read and process the data.

XML is useful when different applications need to exchange information in an agreed structure. It is used in configuration files, document formats and messages between systems.

Its main properties are:

- **Extensibility:** authors define tags suitable for their data.
- **A hierarchical structure:** elements can contain other elements, forming a tree.
- **Human-readable text:** descriptive tags help readers understand the data.
- **Platform independence:** different languages and systems can process the same text format.
- **Separation of data and presentation:** the XML describes the data; other technologies determine how to display it.
- **Validation support:** a DTD or schema can specify the structure that documents must follow.

Custom tags alone do not guarantee that two systems understand each other. They must also agree on the meaning and structure of the data.

## 1.2 XML versus HTML

| Basis | XML | HTML |
| --- | --- | --- |
| Main purpose | Represent, store and exchange structured data. | Describe the structure and meaning of web-page content. |
| Tags | Authors define application-specific tags. | Uses standard elements such as `p`, `h1` and `table`. |
| Meaning of a tag | Agreed by the application, such as `price`. | Defined by HTML, such as a paragraph or a link. |
| Syntax | Requires strict nesting, matching case and quoted attributes. | HTML syntax allows some omitted tags and unquoted attribute values. |
| Error handling | An XML parser reports malformed XML as an error. | Browsers attempt to recover from many HTML errors. |
| Presentation | Has no built-in display meaning for custom tags. | Browsers know how to render standard elements; CSS controls styling. |

## 1.3 Structure of an XML document

```xml
<?xml version="1.0" encoding="UTF-8"?>
<catalogue>
  <!-- A book available in the store -->
  <book id="b1">
    <title>Web Basics</title>
    <author>Asha</author>
    <price>450</price>
  </book>
</catalogue>
```

The document contains the following building blocks:

- The optional **XML declaration** identifies the XML version and character encoding. If used, it appears at the beginning.
- The **root element**, `catalogue`, encloses all other elements.
- An **element** includes its tags and content, such as `<title>Web Basics</title>`.
- An **attribute** supplies information in a start tag, such as `id="b1"`. Attribute values must be quoted.
- **Text content** is the data between tags, such as `Asha`.
- A **comment** is written as `<!-- comment -->` and provides information for readers.

`catalogue` is the parent of `book`. `title`, `author` and `price` are children of `book` and siblings of one another. An element with no content can use the short form `<available/>`, equivalent to `<available></available>`.

An attribute is suitable for a small property such as an identifier. Child elements are useful when information may repeat or have its own internal structure.

### Special characters, entities and CDATA

Some characters have a special meaning in XML. Write `&lt;` for a literal `<` and `&amp;` for a literal `&` in ordinary text. For example:

```xml
<title>Web &amp; Internet</title>
```

An **entity reference** stands for replacement text. XML provides five predefined references: `&lt;`, `&gt;`, `&amp;`, `&quot;` and `&apos;`. Other named entities must be declared.

A **CDATA section** allows text containing characters such as `<` and `&` without treating them as markup:

```xml
<condition><![CDATA[price < 500 && available]]></condition>
```

CDATA changes how text is written, not the underlying meaning of the data. It cannot contain its own closing sequence, `]]>`.

## 1.4 Well-formed XML and valid XML

A **well-formed XML document** follows XML's syntax rules:

1. It has exactly one root element.
2. Each start tag has a matching end tag, unless an empty-element tag is used.
3. Tags are case-sensitive: `<Book>` and `</book>` do not match.
4. Elements are properly nested: an inner element closes before its outer element.
5. Attribute values are quoted, and an attribute name is not repeated within the same start tag.
6. Reserved characters are escaped where required, and names follow XML naming rules. For ordinary names, start with a letter or underscore and do not use spaces.

For example, `<book><title>Web</book></title>` is not well-formed because the tags overlap. The correct nesting is `<book><title>Web</title></book>`.

**Validation** checks whether a well-formed document also satisfies a specified set of rules, such as a DTD or XML Schema. These rules can require particular elements, their order, attributes or data types.

| Well-formed XML | Valid XML |
| --- | --- |
| Follows XML syntax rules. | Follows XML syntax and the applicable validation rules. |
| Can be checked without a DTD or schema. | Requires rules against which to validate the document. |
| Does not guarantee the expected application structure. | Conforms to the structure defined for that application. |

If a DTD requires a book to contain both `title` and `author`, `<book><title>Web</title></book>` is well-formed but invalid against that DTD. **Every valid document must be well-formed, but a well-formed document need not be valid.**

## 1.5 DTD: Defining the permitted document structure

A **Document Type Definition (DTD)** defines which elements and attributes may appear in an XML document and how they may be arranged. It gives applications a common structural agreement: for example, every book must have a title followed by an author.

The syllabus uses “Document Type Declaration (DTD).” Distinguish the two terms: **DTD** is the rule set; **DOCTYPE** is the declaration that associates those rules with the XML document.

### Internal DTD

An internal DTD places the declarations inside square brackets within the document's `DOCTYPE`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE catalogue [
  <!ELEMENT catalogue (book+)>
  <!ELEMENT book (title, author)>
  <!ATTLIST book id ID #REQUIRED>
  <!ELEMENT title (#PCDATA)>
  <!ELEMENT author (#PCDATA)>
]>
<catalogue>
  <book id="b1">
    <title>Web Basics</title>
    <author>Asha</author>
  </book>
</catalogue>
```

Here, `catalogue` must contain one or more books. Each book must contain a title followed by an author. The required `id` attribute has type `ID`, so its value must be a valid XML name and unique within the document. `#PCDATA` means parsed character data: ordinary text whose entity references are processed. In this declaration, `title` and `author` contain text rather than child elements.

### External DTD

An external DTD stores the same declarations in a separate file, such as `catalogue.dtd`. The XML document refers to it as follows:

```xml
<!DOCTYPE catalogue SYSTEM "catalogue.dtd">
```

The external file contains declarations such as `<!ELEMENT catalogue (book+)>`, without the surrounding `DOCTYPE` wrapper. The `DOCTYPE` name must match the root element, here `catalogue`. Several documents can share the same external DTD. An internal DTD is convenient for a self-contained document; an external DTD is easier to reuse and maintain.

### Important DTD declarations and symbols

| Declaration or symbol | Meaning |
| --- | --- |
| `<!ELEMENT title (#PCDATA)>` | `title` contains text. |
| `<!ELEMENT book (title, author)>` | The children must occur in the stated sequence. |
| `(email \| phone)` | Choose one of the listed alternatives. |
| `book?` | Zero or one book. |
| `book*` | Zero or more books. |
| `book+` | One or more books. |
| `<!ELEMENT available EMPTY>` | The element has no content. |
| `<!ATTLIST book edition CDATA #IMPLIED>` | `edition` is an optional text attribute. |
| `#REQUIRED` | The attribute must be supplied. |
| `#IMPLIED` | The attribute may be omitted, with no default supplied. |

The attribute type `CDATA` means a text value; it is different from a CDATA section inside element content. An attribute declaration can also supply a default value, such as `edition CDATA "1"`; `#FIXED "1"` requires that value if the attribute is supplied.

**An attribute named `id` does not automatically have type `ID`.** For XML such as `<book id="2">`, declare it as `CDATA` if the value must remain `2`: the DTD type `ID` does not allow a value beginning with a digit. This matters in the book example from the 2019 paper.

### Mixed content

Mixed content combines ordinary text with child elements. For example:

```xml
<book id="b1">A useful guide: <title>Web Basics</title> by <author>Asha</author></book>
```

Its DTD content declaration can be:

```dtd
<!ELEMENT book (#PCDATA|title|author)*>
```

This permits text and the named child elements in a mixture. It does not enforce exactly one title followed by exactly one author. The child elements and attribute still need their own declarations. This distinction matters when a question gives XML containing text directly inside `book`, rather than only child elements. See the [W3C XML specification](https://www.w3.org/TR/xml/) for the mixed-content rules.

### DTD limitations

DTDs use a separate, non-XML syntax. They can constrain document structure and some attribute values, but cannot directly require element text to be an integer, date or decimal. They also lack namespace-aware validation. XML Schema addresses these limitations.

## 1.6 XML Schema: Checking structure and data types

**XML Schema Definition (XSD)** describes the permitted structure and values of XML documents using XML syntax. For example, a DTD can require a `price` element, while XSD can additionally require its value to be a decimal number.

The main components are:

- **Element and attribute declarations:** specify permitted names and types.
- **Simple types:** describe values without child elements or attributes, such as strings, integers, dates and booleans.
- **Complex types:** describe elements with child elements and/or attributes.
- **Content groups:** `xs:sequence` requires an order; `xs:choice` allows alternatives; `xs:all` permits its declared children in any order, subject to its occurrence rules.
- **Occurrence constraints:** `minOccurs` and `maxOccurs` control repetition. Their defaults are both `1`.
- **Restrictions:** constrain values, for example to a permitted range or a list of choices.

A small schema for a book is shown below. The prefix `xs` identifies names from the XML Schema vocabulary, such as `xs:element` and `xs:string`.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="book">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="title" type="xs:string"/>
        <xs:element name="price" type="xs:decimal"/>
      </xs:sequence>
      <xs:attribute name="id" type="xs:ID" use="required"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

The matching XML can be `<book id="b1"><title>Web Basics</title><price>450.00</price></book>`. A price of `cheap` would fail validation because it is not a decimal value. The validator must be given or directed to the relevant schema.

| Basis | DTD | XSD |
| --- | --- | --- |
| Syntax | Special declaration syntax. | XML syntax. |
| Data types | Limited, particularly for element text. | Rich built-in types and user-defined restrictions. |
| Namespaces | Not namespace-aware. | Supports namespaces. |
| Repetition | Uses symbols such as `?`, `*` and `+`. | Uses explicit minimum and maximum occurrences. |
| Suitable use | Relatively simple structural rules. | Detailed structure and value constraints. |

The example and type distinctions follow the [W3C XML Schema primer](https://www.w3.org/TR/xmlschema-0/).

## 1.7 XML namespaces: Avoiding name conflicts

Two XML vocabularies may use the same name for different things. A book record and a person's record might both contain `title`. A **namespace** distinguishes names by associating them with a URI (Uniform Resource Identifier), a string used to identify the vocabulary.

```xml
<record xmlns:b="urn:books" xmlns:p="urn:people">
  <b:title>Web Basics</b:title>
  <p:title>Professor</p:title>
</record>
```

`xmlns:b` associates the prefix `b` with `urn:books`; `xmlns:p` associates `p` with `urn:people`. Although both local names are `title`, their namespace identifiers differ, so they are distinct names.

The prefix is a convenient label. The namespace identity comes from the URI, which need not be a downloadable web page. A declaration applies within its element and descendants unless overridden.

A **default namespace** applies to unprefixed element names:

```xml
<book xmlns="urn:books" id="b1">
  <title>Web Basics</title>
</book>
```

Here, `book` and `title` belong to `urn:books`. The unprefixed attribute `id` does not: default namespaces do not apply to unprefixed attributes. A namespace prevents naming ambiguity; it does not itself validate the document. These rules are defined in [Namespaces in XML](https://www.w3.org/TR/xml-names/).

## 1.8 XML DOM: Reading and changing XML as a tree

The **Document Object Model (DOM)** represents a document as objects called nodes. An XML parser reads XML text and constructs this tree so that a program can inspect or change it.

For `<book id="b1"><title>Web Basics</title></book>`, the main tree is:

```text
Document
└── book element [attribute: id="b1"]
    └── title element
        └── text: "Web Basics"
```

The document node represents the whole document. Element nodes represent elements, while text nodes hold the text inside them. Attributes are associated with their elements; they are not ordinary child nodes in this tree. Comments can also be nodes.

Typical DOM operations include:

| Task | Example API |
| --- | --- |
| Obtain the root element | `documentElement` |
| Select elements by tag name | `getElementsByTagName("title")` |
| Read or replace text | `textContent` |
| Read or change an attribute | `getAttribute("id")`, `setAttribute("id", "b2")` |
| Navigate relationships | `parentNode`, `childNodes` |
| Create and insert an element | `createElement()`, `appendChild()` |
| Remove a child | `removeChild()` |

For example, in browser JavaScript:

```javascript
const xml = '<book id="b1"><title>Web Basics</title></book>';
const doc = new DOMParser().parseFromString(xml, "application/xml");
const title = doc.getElementsByTagName("title")[0];
console.log(title.textContent);   // Web Basics
title.textContent = "XML Basics";
```

The final statement changes the in-memory tree. It does not automatically rewrite a file on the server. Also, parsing and validation are different tasks: constructing a DOM does not automatically validate the document against a DTD or XSD.

## 1.9 XSL and XSL transformation

XML stores data without defining a presentation for its custom tags. **XSL (Extensible Stylesheet Language)** is a family of technologies used to transform and format XML:

- **XSLT (XSL Transformations):** transforms an XML document into another document, such as HTML, text or XML.
- **XPath (XML Path Language):** selects nodes in an XML tree; XSLT uses it to locate data.
- **XSL-FO (XSL Formatting Objects):** describes formatting for page-based output, such as printed documents.

### How XSLT works

An XSLT stylesheet contains **templates**, which are rules for producing output from selected parts of the input. An XSLT processor applies these rules to the source XML.

```text
Source XML + XSLT stylesheet → XSLT processor → HTML, text or XML output
```

Suppose the source is `<book><title>Web Basics</title></book>`. The following stylesheet produces an HTML heading:

```xml
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="html"/>
  <xsl:template match="/">
    <html><body>
      <h1><xsl:value-of select="book/title"/></h1>
    </body></html>
  </xsl:template>
</xsl:stylesheet>
```

`match="/"` matches the document root. `book/title` is an XPath expression selecting the title, and `xsl:value-of` places its text into the output. The result contains `<h1>Web Basics</h1>`. Other useful instructions include `xsl:for-each` to process selected nodes repeatedly and `xsl:apply-templates` to apply template rules to them. See the [XSLT specification](https://www.w3.org/TR/xslt.html).

### XSLT versus CSS

CSS controls appearance, such as fonts, colours and spacing. XSLT can change the output structure: select fields, rearrange data, sort records or generate an HTML table. CSS can then style that generated HTML.

For direct XML styling, a stylesheet can be associated using `<?xml-stylesheet type="text/css" href="style.css"?>`. The key distinction is **styling the existing content versus transforming it into a different output structure**.

# 2. AJAX: Communicating with the server without reloading the page

## 2.1 What AJAX means

**AJAX (Asynchronous JavaScript and XML)** is a technique for exchanging data with a server and updating part of a web page without reloading the whole page. For example, a search box can request suggestions while the user types.

AJAX is not a programming language. JavaScript is the language used to coordinate requests and update the page. Despite the name, AJAX does not require XML: responses may contain JSON, XML, HTML or plain text.

The technologies involved have different responsibilities:

- **HTML and CSS** provide page structure and appearance.
- **JavaScript** reacts to user actions and processes responses.
- **XMLHttpRequest (XHR)** provides an interface for making HTTP requests.
- **The DOM** allows JavaScript to update the displayed page.
- **Server-side software** processes requests and, if necessary, accesses files or a database.

## 2.2 AJAX client-server architecture and working steps

```text
Browser                                      Server
User action
    ↓
JavaScript creates and sends an HTTP request ──→ Process request
                                                ↓
                                             Read file/query database
                                                ↓
Response handler receives data              ←── Send response
    ↓
JavaScript updates the required DOM elements
```

The working sequence is:

1. A user action, such as typing or selecting an item, triggers JavaScript.
2. JavaScript creates an XHR object and registers a response handler.
3. It configures and sends the HTTP request.
4. The server processes the request and returns a response.
5. The handler checks whether the request succeeded and reads the returned data.
6. JavaScript changes the relevant part of the page through the DOM.

### Traditional requests and AJAX updates

| Traditional full-page request | AJAX request |
| --- | --- |
| Usually returns a whole new page. | Returns data or a fragment needed for an update. |
| Replaces or reloads the displayed page. | Changes selected elements in the existing page. |
| Can interrupt the user's current interaction. | Can preserve the current page and input. |
| Server generates the new page. | Client-side code also participates in processing and display. |

In older exam terminology, a **full postback** submits a page/form and refreshes the whole page. A **partial or asynchronous postback** sends a request and updates only part of the page. “Postback” is framework terminology; it is not a separate XHR state, nor does every AJAX request have to use HTTP POST.

### Advantages and limitations

AJAX can make interfaces more responsive, preserve page state and reduce repeated transfer of unchanged page content. Typical uses include search suggestions, live validation and loading details for a selected item.

However, it requires JavaScript and careful handling of delays, errors and loading states. Frequent requests can increase server load. Reading an AJAX response from a different origin (a different scheme, host or port) requires permission from that server through **CORS (Cross-Origin Resource Sharing)**. AJAX does not remove network delays; it allows the interface to continue while waiting.

## 2.3 Synchronous requests, asynchronous requests and callbacks

A **synchronous request** blocks the calling code until the response is available. On the browser's main thread, this can make the interface unresponsive.

An **asynchronous request** lets the calling code continue. The browser invokes a registered function, called an event handler, as the request progresses or finishes. This is the normal approach for AJAX.

A **callback** is a function supplied to be called later. A response callback processes the result when it is available. Code immediately after `send()` may run before the response arrives, so it cannot assume that the data is ready. The [MDN explanation of synchronous and asynchronous requests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest_API/Synchronous_and_Asynchronous_Requests) illustrates this distinction.

## 2.4 The XMLHttpRequest object

Create an XHR object with `new XMLHttpRequest()`. Its methods configure and send a request, while its properties describe the request state and response.

### Important methods

| Method | Purpose |
| --- | --- |
| `open(method, url, true)` | Configures the HTTP method, URL and asynchronous operation. It does not send the request. |
| `send()` or `send(body)` | Sends the request, optionally with a body. |
| `setRequestHeader(name, value)` | Sets a request header after `open()` and before `send()`. |
| `getResponseHeader(name)` | Reads a response header, such as `Content-Type`. |
| `abort()` | Cancels the request. |

### Important properties and handlers

| Member | Meaning |
| --- | --- |
| `readyState` | The stage of the request, numbered 0–4. |
| `status` | HTTP response code, such as 200 or 404. |
| `statusText` | HTTP status description, when available. |
| `responseText` | The response body as text. |
| `responseXML` | The response parsed as a document when appropriate; it can be `null`. |
| `onreadystatechange` | Handler called as the ready state changes. |
| `onerror` | Handler for a network-level failure. |

These names and roles follow the [XMLHttpRequest standard](https://xhr.spec.whatwg.org/).

## 2.5 Ready states and server responses

| Value | Name | Meaning |
| --- | --- | --- |
| 0 | UNSENT | The object exists, but `open()` has not been called. |
| 1 | OPENED | `open()` has configured the request. |
| 2 | HEADERS_RECEIVED | The request has been sent and response headers are available. |
| 3 | LOADING | Response data is being received. |
| 4 | DONE | The operation has finished, successfully or unsuccessfully. |

**Ready state and HTTP status answer different questions.** `readyState === 4` means the operation is finished. A successful HTTP status, such as `200 OK`, indicates that the server handled the request successfully. A completed request may instead return `404 Not Found` or `500 Internal Server Error`.

### A small AJAX example

Assume the page contains `<p id="result">Loading...</p>` and the server provides a text file named `message.txt`.

```javascript
const xhr = new XMLHttpRequest();
const result = document.getElementById("result");

xhr.onreadystatechange = function () {
  if (xhr.readyState !== 4) return;

  if (xhr.status >= 200 && xhr.status < 300) {
    result.textContent = xhr.responseText;
  } else {
    result.textContent = "The request failed.";
  }
};

xhr.open("GET", "message.txt", true);
xhr.send();
```

The callback runs when the ready state changes. It ignores unfinished states, then checks the HTTP status and displays either the response or an error message. The third argument to `open()` enables asynchronous operation. `textContent` displays the returned value as text rather than interpreting it as HTML.

### GET versus POST

| Basis | GET | POST |
| --- | --- | --- |
| Usual purpose | Retrieve a resource without requesting a change to server state. | Submit data for processing, often creating or changing something. |
| Data location | Parameters commonly appear in the URL query string. | Submitted data commonly appears in the request body. |
| Example | `books?id=b1` | Sending the fields of a new book record. |
| Caching/bookmarking | URLs can be bookmarked; responses may be cached. | Submitted bodies are not captured by bookmarks; responses are not normally cached without explicit support. |

POST is not automatically secure or encrypted. HTTPS provides transport encryption. In XHR, a POST body may require a suitable `Content-Type` header so the server knows how to interpret it.

## 2.6 AJAX with XML

When a server returns XML, JavaScript can read the parsed document through `responseXML`, select the required elements and display their values.

For a successful response containing `<book><title>Web Basics</title></book>`, the response-handling step could be:

```javascript
const doc = xhr.responseXML;
if (doc) {
  const title = doc.getElementsByTagName("title")[0];
  if (title) result.textContent = title.textContent;
}
```

This fragment belongs inside the successful-response branch. The server should identify XML using an appropriate response content type, such as `application/xml`.

There are two distinct flows worth recognizing:

- **XML returned to the browser:** the browser receives XML, reads its DOM and chooses what to display.
- **XML read on the server:** the server reads an XML file, selects data and returns HTML or text. The teacher's CD-catalogue example follows this pattern, so the browser uses `responseText` rather than `responseXML`.

The format stored on the server need not be the format returned to the browser.

## 2.7 AJAX with a database

AJAX can display database information without reloading the page. For example, selecting a student ID can load that student's details.

1. JavaScript sends the selected ID to a server URL that handles the lookup.
2. Server-side code validates the input and queries the database.
3. The server formats the result as JSON, XML, HTML or text.
4. It sends that result in the HTTP response.
5. JavaScript updates the details area of the page.

**The browser communicates with the server, not directly with the database.** The server owns the database connection and query logic. AJAX controls the request and page update; it is not itself a database-access language.

# 3. jQuery: Selecting and changing web-page elements

## 3.1 Purpose and basic syntax

**jQuery is a JavaScript library** that provides convenient methods for selecting DOM elements, reading and changing content, managing styles and handling events. It also provides AJAX helpers. JavaScript is the underlying language; jQuery supplies reusable functions written for that language.

Its advantages include concise DOM operations, familiar CSS selector syntax, method chaining and a consistent interface across the browsers supported by the chosen version. It reduces repetitive code but is not required to use JavaScript or AJAX.

The basic pattern is:

```javascript
$(selector).method(arguments);
```

`$` is the usual alias for `jQuery`. A selector identifies elements, and the returned **jQuery object** wraps the matched collection so that jQuery methods can operate on it. For example, `$("p").addClass("important")` adds a class to all selected paragraphs.

Load the jQuery library before code that uses it. When code depends on page elements, run it after the DOM is ready:

```javascript
$(function () {
  // The document's DOM is ready to use.
});
```

This is the concise form of `$(document).ready(function () { ... });`. DOM readiness does not mean that every image has finished loading.

## 3.2 DOM selectors

Selectors choose the elements on which an operation will act.

| Selection | Example | Meaning |
| --- | --- | --- |
| Element | `$("p")` | All paragraphs. |
| ID | `$("#message")` | The element with ID `message`. |
| Class | `$(".notice")` | Elements with class `notice`. |
| Element and class | `$("p.notice")` | Paragraphs with class `notice`. |
| Attribute present | `$("a[href]")` | Links having an `href` attribute. |
| Attribute value | `$('input[type="text"]')` | Text inputs. |
| Descendant | `$("#menu a")` | Links anywhere inside `menu`. |
| Direct child | `$("#menu > a")` | Links immediately inside `menu`. |
| Group | `$("h1, h2")` | Both heading levels. |

Selections can be narrowed using methods. `$("li").first()` chooses the first selected list item; `$("li").eq(1)` chooses the second because indexing starts at zero. `$("p").filter(".notice")` retains only the selected paragraphs with that class.

Inside an ordinary event-handler function, `$(this)` wraps the element handling the event. It avoids selecting every element when only the current one should change.

## 3.3 Reading and changing HTML content

A **getter** reads a value; a **setter** changes it. Many jQuery methods serve both roles, depending on the arguments supplied.

| Method | Getter example | Setter example |
| --- | --- | --- |
| `text()` | `$("#message").text()` reads text. | `$("#message").text("Saved")` replaces text. |
| `html()` | `$("#message").html()` reads inner HTML. | `$("#message").html("<b>Saved</b>")` replaces inner HTML. |
| `val()` | `$("#name").val()` reads an input's current value. | `$("#name").val("Asha")` changes it. |
| `attr()` | `$("#link").attr("href")` reads an attribute. | `$("#link").attr("href", "books.html")` changes it. |

`text("<b>Saved</b>")` displays the angle brackets and tags as text. `html("<b>Saved</b>")` interprets them as markup and displays a bold word. Prefer `text()` when displaying ordinary user-provided text.

Setters generally affect every matched element. Getters such as `html()`, `val()` and `attr()` usually read from the first matched element; `text()` combines the text of all matched elements. Use `val()` for form values, not `text()`. See the [jQuery text documentation](https://api.jquery.com/text/).

## 3.4 Changing CSS styles and classes

`css()` reads a computed style or sets a style:

```javascript
const colour = $("p").css("background-color"); // First paragraph's value.
$("p").css("color", "red");                  // Set all paragraphs.
$("#message").css({ color: "blue", "font-weight": "bold" });
```

Reading a computed colour may return an `rgb(...)` string rather than the original colour name.

Classes let a stylesheet define a reusable appearance while JavaScript decides when it applies.

| Method | Purpose |
| --- | --- |
| `addClass("notice")` | Adds the class without removing other classes. |
| `removeClass("notice")` | Removes the named class. |
| `toggleClass("notice")` | Adds it if absent; removes it if present. |
| `hasClass("notice")` | Reports whether any selected element has the class. |

For example, with `.notice { color: red; }` in the stylesheet, `$("#message").addClass("notice")` makes the message red. `css()` sets a style directly; class methods change which CSS rules apply.

## 3.5 Creating, inserting and removing DOM elements

Changing the DOM's structure means adding or removing elements, rather than merely changing their existing text or colour.

| Method | Effect relative to the selected element |
| --- | --- |
| `append(content)` | Inserts content inside, after existing children. |
| `prepend(content)` | Inserts content inside, before existing children. |
| `before(content)` | Inserts content immediately before the element, as a sibling. |
| `after(content)` | Inserts content immediately after the element, as a sibling. |
| `empty()` | Removes the element's contents but keeps the element itself. |
| `remove()` | Removes the element and its contents. |

For example:

```javascript
const item = $("<li>").text("Web Basics");
$("#books").append(item);
```

This creates an `li` element, gives it text and adds it inside the list with ID `books`.

**Traversal** means moving through DOM relationships. `parent()` selects immediate parents; `children()` selects direct element children; `find("a")` searches descendants for matching links. Thus, `$("#menu").children("a")` and `$("#menu").find("a")` differ when links are nested inside other elements.

### Method chaining

Many setters return the jQuery object, allowing several operations on the same selection:

```javascript
$("#message").text("Saved").addClass("notice");
```

A getter returns data instead, so it does not generally continue the same chain of DOM operations.

## 3.6 Events and a small complete interaction

An **event handler** is a function that runs when an event occurs. The usual general form is `$(selector).on("event", handler)`.

```html
<button id="save">Save</button>
<p id="message"></p>
```

```javascript
$(function () {
  $("#save").on("click", function () {
    $("#message").text("Saved").css("color", "green");
  });
});
```

The ready callback waits for the DOM. The click handler then changes the message only when the button is clicked. The library must already be loaded.

Older PYQs name these event methods:

| Name | Meaning |
| --- | --- |
| `ready(handler)` | Runs when the DOM is ready to be used. |
| `mouseenter(handler)` | Runs when the pointer enters the element. |
| `mousedown(handler)` | Runs when a mouse button is pressed while the pointer is over the element. |
| `hover(enterHandler, leaveHandler)` | Attaches handlers for entering and leaving the element. |

Recognize their meaning for theory answers. Use `.on("mouseenter", handler)`, `.on("mousedown", handler)` and separate enter/leave handlers in new examples; `hover()` is a deprecated convenience method. See the official [on](https://api.jquery.com/on/) and [hover](https://api.jquery.com/hover/) documentation.

### Why `noConflict()` is used

Another library may also use `$`. `jQuery.noConflict()` releases jQuery's control of that alias, allowing its previous owner to use it. jQuery remains available through its full name or a chosen alias:

```javascript
const jq = jQuery.noConflict();
jq("#message").text("Saved");
```

This resolves an alias conflict; it does not unload jQuery. See [jQuery.noConflict](https://api.jquery.com/jquery.noConflict/).

# 4. JSON: Representing and exchanging data

## 4.1 What JSON is and why it is useful

**JSON (JavaScript Object Notation)** is a text format for representing structured data. It is commonly used for API responses, configuration and communication between applications.

Although its notation comes from JavaScript, JSON is language-independent. A server written in one language can produce JSON and a client written in another can parse it. JSON describes values; it is not executable program code or a markup language.

Its compact structure makes it convenient for data such as books, students and orders. In AJAX, the server can return JSON, and the browser can convert it into JavaScript values before updating the page.

## 4.2 Syntax rules and data types

JSON uses objects and arrays to group data:

- An **object** uses braces `{}` and contains name/value pairs. Each name is a double-quoted string, followed by a colon and its value.
- An **array** uses brackets `[]` and contains an ordered sequence of values.
- Commas separate object members or array values. There is no trailing comma.
- Strings and property names use double quotes, not single quotes.
- `true`, `false` and `null` are lowercase and unquoted.
- Standard JSON does not allow comments, functions or `undefined`.

| Type | Example | Meaning |
| --- | --- | --- |
| String | `"Web Basics"` | Text; quotes and special characters must be escaped where needed. |
| Number | `450`, `4.5` | A numeric value, without quotes. |
| Boolean | `true` | A logical true/false value. |
| Null | `null` | An explicitly absent or empty value. |
| Object | `{"name": "Asha"}` | A collection of named values. |
| Array | `["XML", "AJAX"]` | An ordered collection of values. |

Numbers can also use exponent notation. Values such as `NaN` and `Infinity` are not valid JSON numbers. JSON has no separate date type; a date is commonly represented as a string such as `"2026-11-15"`.

A complete JSON text can represent any JSON value, although objects and arrays are the usual forms for records and collections.

### Common syntax mistakes

| Invalid form | Correct form | Reason |
| --- | --- | --- |
| `{name: 'Asha'}` | `{"name": "Asha"}` | Names and strings need double quotes. |
| `{"active": True}` | `{"active": true}` | Boolean literals are lowercase. |
| `{"price": 450,}` | `{"price": 450}` | Trailing commas are not allowed. |

`{"price": "450"}` is valid JSON, but the value is a string rather than a number. Valid syntax alone does not guarantee that the application receives the expected type.

## 4.3 Objects, arrays and nested data

An object groups related named properties. An array groups a sequence of values, which may themselves be objects or arrays.

```json
{
  "store": "Campus Books",
  "books": [
    {"title": "Web Basics", "author": "Asha", "topic": "XML", "edition": 2},
    {"title": "Web Data", "author": "Ravi", "topic": "JSON", "edition": 1}
  ],
  "open": true,
  "discount": null
}
```

The outer object represents the store. Its `books` property contains an array of book objects. Each object keeps the information for one book together. A five-book catalogue uses the same structure with five objects; no new syntax is required.

Object members are identified by name; do not depend on their written order as the meaning of the data. Array order is meaningful, and JavaScript array indexes start at zero.

Once this JSON text has been parsed into a JavaScript value called `data`, values can be accessed as follows:

```javascript
data.store;               // "Campus Books"
data["books"][0].author;  // "Asha"
data.books[1].edition;    // 1
```

These are **JavaScript expressions operating on parsed data**, not statements that belong inside a JSON file.

## 4.4 Parsing and stringifying

JSON text and a JavaScript object are different representations. Text is suitable for transmission or storage; an in-memory object is convenient for computation and property access.

| Operation | Input | Output | Typical use |
| --- | --- | --- | --- |
| `JSON.parse(text)` | Valid JSON text in a string. | The corresponding JavaScript value. | Read a server response. |
| `JSON.stringify(value)` | A supported JavaScript value. | Its JSON text representation. | Prepare data for sending or storage. |

```javascript
const received = '{"title":"Web Basics","edition":2}';
const book = JSON.parse(received);

console.log(book.title);             // Web Basics
book.edition = 3;

const outgoing = JSON.stringify(book);
console.log(outgoing);               // {"title":"Web Basics","edition":3}
```

Parsing converts the text into data; it does not merely remove quotation marks. Stringifying serializes a value: it produces a textual representation that can be reconstructed later.

### Errors and limitations to remember

`JSON.parse()` throws a `SyntaxError` if the text is not valid JSON. For external input, handle the error rather than assuming parsing always succeeds:

```javascript
try {
  const data = JSON.parse(received);
  console.log(data.title);
} catch (error) {
  console.log("The response is not valid JSON.");
}
```

Parsing checks JSON syntax, not application requirements. A valid JSON value could still lack the required `title` property.

Stringifying is not a way to preserve every JavaScript feature. Functions and `undefined` properties are omitted from objects; such entries in arrays become `null`. Circular references cannot be directly stringified and cause an error. Ordinary JSON-compatible records and arrays are the intended use here. These conversion behaviours are defined by the [ECMAScript JSON specification](https://tc39.es/ecma262/multipage/structured-data.html#sec-json-object).

### JSON in an AJAX response

After confirming that a text response succeeded, the browser can use `JSON.parse(xhr.responseText)` to obtain data and then update the page. In the other direction, `JSON.stringify(data)` prepares JSON text for a request body, with `Content-Type: application/json` identifying its format.

AJAX handles the communication; JSON supplies a data representation. A server response does not become visible on the page merely because it contains JSON: JavaScript must decide how to display it.

## 4.5 JSON versus XML

Both are text-based, language-independent formats that can represent nested data and be processed by many programming languages.

| Basis | JSON | XML |
| --- | --- | --- |
| Structure | Objects, arrays and values. | Elements, attributes and text. |
| Notation | Braces, brackets and name/value pairs. | Start/end tags and attributes. |
| Size for ordinary records | Often more compact. | Repeated tags can make it more verbose. |
| Data types | Includes numbers, booleans and null directly. | Element text needs interpretation or schema-defined typing. |
| Collections | Has a native array notation. | Commonly uses repeated elements. |
| Namespaces | No built-in namespace mechanism. | Has a standard namespace mechanism. |
| Validation | Can use a separate system such as JSON Schema. | Can use DTD or XSD. |
| Document content | Convenient for structured records. | Suits documents that mix text and markup. |
| Transformation | Usually processed with application code. | Has established technologies such as XSLT and XPath. |

For the same simple record, XML might be `<book><title>Web Basics</title><edition>2</edition></book>`, while JSON is `{"title":"Web Basics","edition":2}`.

**Why is JSON often preferred for message exchange?** It is usually concise for ordinary records, maps naturally to objects and arrays, carries primitive types directly, and is easy to parse/stringify in JavaScript. These are four useful reasons for an exam answer.

The choice depends on the data and the application. XML remains useful when namespaces, document markup or XML transformations are important.

# 5. Final revision check

Use these prompts to check whether you can explain the material without merely recalling definitions:

- **XML:** Can you label a document's building blocks, compare XML with HTML, distinguish well-formedness from validity, write a short DTD and matching XML, and explain namespaces, schema, DOM and XSLT?
- **AJAX:** Can you draw the browser-server flow, distinguish synchronous and asynchronous requests, explain callbacks, list all five ready states, separate readyState from status, and explain XML/database responses?
- **jQuery:** Can you choose a selector, distinguish text/HTML/input values, read and change CSS, add a class, insert/remove elements, and explain readiness, events and noConflict?
- **JSON:** Can you identify the six types, correct invalid syntax, represent nested records, explain parse versus stringify, and compare JSON with XML?

Repeated PYQ emphasis includes DTD and XML validity, AJAX ready states/callbacks, jQuery selectors/styles, and JSON syntax/comparisons. The other syllabus topics remain examinable. For a theory answer, begin with the purpose, explain the mechanism or rules, and add a small example or comparison that directly supports the question.

## Source basis

Coverage follows the [Module III syllabus, page 3](../sources/syllabus/IWT-7th-Semester-IT.pdf#page=3) and the supplied exam exclusions. The teacher's [XML](<../sources/sir's notes/XML.pdf>), [AJAX](<../sources/sir's notes/AJAX.pdf>), [jQuery](<../sources/sir's notes/jQuery.pdf>) and [JSON](<../sources/sir's notes/JSON.pdf>) PDFs provided the reference material. The [coverage plan](module-3/coverage.md) records question-level references from all five supplied PYQ files. Technical documentation is linked where it clarifies gaps or corrects misleading source examples.
