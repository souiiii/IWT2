# Module III coverage plan and completion record

Scope: XML, AJAX, jQuery, and JSON. React.js and Node.js are excluded. All 28 included syllabus entries are covered in [notes/module-3.md](../module-3.md), the single study-notes file. The original four-file proposal has been superseded.

Authority: [exam instructions](../../sources/exam-instructions.md) and [syllabus, PDF page 3](../../sources/syllabus/IWT-7th-Semester-IT.pdf#page=3). Every included syllabus entry is mapped below. PYQs influence emphasis, never eligibility.

## Priorities and answer depth

**High** means extra revision emphasis because of repeated/direct PYQs or a concept's central role. **Core** means required syllabus coverage with a focused explanation. Neither label permits omission. **Brief support** means a small supporting explanation, not a new chapter.

For a substantial topic, retain enough material for a coherent answer: purpose, mechanism or rules, a useful comparison, and one small example where needed. Explain methods by what they do, not just their names. Older programming questions justify short illustrative snippets, not full applications under the current mostly-theoretical instruction.

## PYQ reference key

Page numbers refer to PDF pages. Years are taken from visible paper headings where available.

| Key | Source and relevant questions |
| --- | --- |
| A | [April 2019, CS201](../../sources/pyqs/CS201_Internet_and_Web_Technology_April_2019.pdf): p. 1, Q2.B.i: DTD and a book XML example with an attribute and mixed content (3 marks). P. 2, Q4.A: AJAX, implementation steps, XMLHttpRequest properties and methods (5 marks). This is a different semester/branch, so use only overlapping topics. |
| B | [2021-labelled, 2018–22 batch](<../../sources/pyqs/IWT 2021 Endsem (2018-22 batch).pdf>): p. 3, Q5.i–iii: pizza-order DTD, matching XML, and XML building blocks; Q5.iv–v: JSON and four reasons to prefer it for message exchange. |
| C | [2021-labelled, 2019–23 batch](<../../sources/pyqs/IWT 2021 Endsem (2019-23 batch).pdf>): p. 2, Q3.i: well-formed versus valid XML, DTD types and example; Q3.ii: book-catalogue DTD, matching XML, and document structure. |
| D | [Collected papers](../../sources/pyqs/IWT_II_Previous_Year_Question_Papers.pdf): November 2023, p. 1, Q1(b): AJAX ready states (2 marks); Q2(b): synchronous/asynchronous requests, callback and ready states (5); Q1(e): jQuery ready, mouseenter, mousedown and hover (2); Q4(b)(iii–iv): JSON/AJAX short notes (2.5 each). P. 2, Q5: travel form using HTML/CSS/jQuery (10). December 2022, p. 3, Q3(i–ii): AJAX application and GET/POST (6+4); Q4(i–ii): adding a CSS class, changing font colour, noConflict and jQuery advantages (6+4); Q5(ii): XML versus JSON (4). |
| E | [2023-labelled collection](../../sources/pyqs/IWT_Question_Papers_2023.pdf): p. 3, undated IWT-2 paper, Q1(a–c): AJAX versus JavaScript, jQuery DOM selectors, JSON syntax/types (2 each); Q2: JSON in AJAX, postbacks and AJAX client-server architecture (10); Q3: JSON applications and a five-book representation (10); Q4: jQuery built-in functions and reading background colour (10). |

**Source cautions:** E p. 1 duplicates D p. 1; count it once. E p. 2 is an unrelated Android paper, not XML syllabus evidence. E p. 3 has no visible date, so do not assign it to 2023 solely from its filename. The two 2021 filenames are retained as labels, not independently verified exam dates. C p. 5 is blank and p. 4 ends mid-question; the relevant XML question on p. 2 is readable. Older papers contain many topics outside the current syllabus.

## Completed coverage checklist

Checked entries have been reviewed against the completed notes. Section numbers refer to `notes/module-3.md`; the descriptions record the coverage criteria and depth to preserve in future edits.

### XML — sections 1.1–1.9

Reference: [XML.pdf](<../../sources/sir's notes/XML.pdf>), all 15 pages; especially pp. 6–11 for structure, namespaces, DTD and schema, and pp. 12–15 for presentation/XSL.

1. **What XML is and how it differs from HTML**
   - [x] Introduction to XML (§1.1) — **Core**. Purpose, custom tags, structured data, useful properties and a few applications. B Q5.i–iv provides context.
   - [x] XML versus HTML (§1.2) — **Core**. Compare purpose, tags, structure and syntax in a small table. No direct question found in the supplied set; still required.
2. **XML document structure and well-formedness**
   - [x] Structure of an XML document (§1.3) — **High**. Declaration, root, elements, attributes, text, comments, entities and parent/child relationships. Briefly distinguish text/CDATA and empty elements. Use one annotated book example. A Q2.B.i; B Q5.i–iii; C Q3.ii.
   - [x] Well-formed XML documents (§1.4) — **High**. Explain the syntax rules and show a small error/correction. C Q3.i.
3. **Validation, DTD, and XML Schema**
   - [x] XML validation (§1.4) — **High**. Explain what a validator checks and why a shared document contract matters. C Q3.i–ii.
   - [x] Valid XML document (§1.4) — **High**. Distinguish syntax correctness from conformance to a DTD/schema, with a well-formed but invalid case. C Q3.i.
   - [x] Document Type Declaration (DTD) (§1.5) — **High**. Clarify the syllabus terminology: DTD means Document Type Definition; DOCTYPE is the declaration associating it with XML. Explain internal/external DTDs, ELEMENT and ATTLIST, text and mixed content, sequence/choice, and `?`, `*`, `+`. Include one short matching XML/DTD pair and external-reference syntax. A Q2.B.i; B Q5.i–iii; C Q3.i–ii.
   - [x] XML Schema (§1.6) — **Core**. Purpose, basic types, simple/complex types, elements/attributes, sequence and occurrence constraints; one small XSD example and DTD-versus-XSD comparison. No direct PYQ found.
4. **XML namespaces**
   - [x] XML Namespaces (§1.7) — **Core**. Name collisions, URI identifiers, prefixes, default namespaces and scope; one collision/resolution example. Briefly clarify the treatment of unprefixed attributes. No direct PYQ found.
5. **Reading and changing XML with the DOM**
   - [x] XML DOM (§1.8) — **Core**. Document/element/text/attribute nodes, tree relationships, parsing, selecting, reading, changing, adding and removing nodes. One tiny tree and access example; no full parser program. No direct PYQ found.
6. **XSL and XML transformation**
   - [x] XSL (§1.9) — **Core**. Purpose and relationship of XSLT, XPath and XSL-FO; keep XPath and XSL-FO to supporting roles.
   - [x] XSL Transformation (§1.9) — **Core**. XML + stylesheet + processor → output; template matching and value selection in one tiny XML-to-HTML example. Briefly contrast CSS styling with transformation. No direct PYQ found for either XSL entry.

### AJAX — sections 2.1–2.7

Reference: [AJAX.pdf](<../../sources/sir's notes/AJAX.pdf>), all 11 pages; pp. 1–4 for the basic flow, pp. 4–7 for XML-backed responses, pp. 7–11 for database-backed responses.

1. **What AJAX does and how it works**
   - [x] AJAX introduction (§2.1–2.3) — **High**. Meaning, participating technologies, AJAX versus JavaScript, traditional versus partial updates, benefits and practical limitations. Explain synchronous/asynchronous requests and callbacks. Include a simple browser/server flow diagram and a short use case. A Q4.A; D Q2(b), Q4(b)(iv), December Q3(i); E Q1(a), Q2.
2. **The XMLHttpRequest object**
   - [x] XMLHttpRequest (§2.4–2.5) — **High**. Creation and request lifecycle; `open`, `send`, request headers and a brief GET/POST comparison. Include a compact methods/properties table and one annotated request snippet. A Q4.A; D December Q3(ii).
3. **Understanding the server response**
   - [x] AJAX server response (§2.5) — **High**. Ready states 0–4, `onreadystatechange`, HTTP status versus readyState, success/error handling, `responseText` versus `responseXML`, and updating the page. D Q1(b), Q2(b); A Q4.A.
4. **Using AJAX with XML**
   - [x] AJAX XML (§2.6) — **Core**. Fetch XML, access its DOM and display selected values. Also explain the reference PDF's different flow: server reads XML and returns HTML. Do not confuse storage format with response format. No direct standalone PYQ found.
5. **Using AJAX with a database**
   - [x] AJAX database (§2.7) — **Core**. Browser request → server-side query → response → page update. Explain responsibilities and one lookup example; the browser does not directly query the database. No direct standalone PYQ found.

Covered in §§2.2 and 4.4: JSON as an alternative response format (E Q2; see §4.4). Full versus partial postback is explained briefly for E Q2, without introducing an ASP.NET chapter.

### jQuery — sections 3.1–3.6

Reference: [jQuery.pdf](<../../sources/sir's notes/jQuery.pdf>), all 16 pages; pp. 3–5 and 8–14 are the main relevant material.

1. **jQuery's purpose and basic syntax** — Brief support: library versus language, benefits, `$`, selected collections, document readiness and chaining. D December Q4(ii); E Q4.
2. **Selecting DOM elements**
   - [x] jQuery DOM selectors (§3.2) — **High**. Element, ID, class, attribute, descendant/child and a few useful filtering examples. Explain what each selects; avoid an exhaustive selector catalogue. E Q1(b).
3. **Reading and changing HTML content**
   - [x] HTML content (§3.3) — **High**. Getter/setter forms of `text`, `html`, `val` and `attr`; distinguish text, markup, form values and attributes. D Q5 and E Q4 provide application context, not a separate demand for full forms.
4. **Changing styles and classes**
   - [x] jQuery CSS styles (§3.4) — **High**. Reading/setting `css`; `addClass`, `removeClass`, `toggleClass` and `hasClass`. Show a tiny colour/class example. D Q5, December Q4(i); E Q4.
5. **Changing the HTML DOM**
   - [x] jQuery HTML DOM (§3.5) — **Core**. Create/insert elements (`append`, `prepend`, `before`, `after`), remove/empty content, and basic parent/children/find traversal. Clarify how this differs from merely changing text or style. E Q4 is broad supporting evidence.

Covered briefly in §§3.1 and 3.6: ready/click and the mouse events named in D Q1(e); one event-driven content/style example. Includes a short explanation of `noConflict` for D December Q4(i). Events and noConflict are not separate named syllabus units; do not expand them into a full API chapter.

### JSON — sections 4.1–4.5

Reference: [JSON.pdf](<../../sources/sir's notes/JSON.pdf>), all 33 pages; pp. 2–3, 7–22 and 24–31 are relevant after removing repetition.

1. **What JSON is and where it is used**
   - [x] Introduction (§4.1, 4.5) — **High**. Text-based data interchange, language independence and use with AJAX. Brief XML-versus-JSON comparison, with context rather than claims that one is always better. B Q5.iv–v; D Q4(b)(iii), December Q5(ii); E Q2–3.
2. **JSON syntax and data types**
   - [x] Syntax (§4.2) — **High**. Names/values, quotes, separators, braces/brackets, nesting and common invalid forms. E Q1(c).
   - [x] Data types (§4.2) — **High**. String, number, boolean, null, object and array; briefly identify unsupported values. E Q1(c).
3. **Objects and arrays**
   - [x] Object (§4.3) — **High**. Named properties, nested objects and accessing values after parsing.
   - [x] Arrays (§4.3) — **High**. Ordered values, indexing, arrays of objects and nesting. Use one small book collection for both entries; explain how it extends to the five-book PYQ without repeating records. E Q3.
4. **Parsing and stringifying JSON**
   - [x] Parsing (§4.4) — **Core**. JSON text → JavaScript value using `JSON.parse`; purpose, input/output and invalid-text errors.
   - [x] Stringify (§4.4) — **Core**. JavaScript value → JSON text using `JSON.stringify`; purpose and input/output. Pair both operations in one small round-trip example and comparison. Explain JSON text versus a JavaScript object. No direct PYQ found for either operation; both are explicitly required.

## Source material retained, shortened or supplemented

| Material | Treatment in the completed notes |
| --- | --- |
| Repeated XML introductions, benefits and syntax | Consolidated into §§1.1–1.4, with small book examples. |
| XML editor instructions and general CSS | Omitted. §1.9 retains the relevant styling/transformation comparison; XPath and XSL-FO remain brief. |
| Gaps in XML DOM, XSLT, DTD and XSD examples | Filled in §§1.5–1.9. The DTD section also explains mixed content and the numeric `id` issue in A Q2.B.i. |
| Long AJAX HTML/PHP/MySQL programs and legacy ActiveX branches | Replaced with a short XHR example and clear XML/database flows. §§2.4–2.5 supply the full ready-state table and response explanations. |
| jQuery effects, obsolete selector syntax and source typos | Omitted or corrected. §§3.2–3.6 explain the required selection/content/style/DOM methods; events and noConflict receive brief PYQ-driven coverage. |
| Repetitive JSON slides, IBM MQ details, Java setup and query-language surveys | Omitted or consolidated. §4.4 fills the missing parse/stringify coverage. |
| Misleading source simplifications | Notes distinguish empty XML elements from unclosed tags, parsing from validation, AJAX from its response format, and JSON text from JavaScript objects. They do not claim that JSON cannot be validated. |
| Older PYQs outside the current scope | React/Node, Java/JDBC/Servlets, enterprise frameworks, cloud topics, SPA and Android are excluded. |

## Scope judgments and final review

- The syllabus uses broad jQuery labels, especially “HTML Content” and “HTML DOM.” The completed notes cover content/value access, styling, structural changes and basic traversal. No exhaustive method list is implied.
- “Mostly theoretical” does not specify a coding proportion or marks distribution. Retain small examples sufficient to explain syntax and mechanisms; do not predict exact marks or reproduce the older application-building workload.
- “Types of postbacks” in E Q2 is not defined by the supplied notes. §2.2 explains full/partial postback briefly as framework terminology, without adding an ASP.NET unit.
- There is no direct PYQ in this supplied set for several required topics, including namespaces, schema, XML DOM, XSL/XSLT, AJAX XML/database and JSON conversion. All are covered in the notes despite the absence of a direct PYQ.

Final review: all 28 checklist entries map to the single notes file. The worked DTD/XSD and XML/JSON examples were checked during drafting; this refinement preserves them and clarifies the DTD attribute-type choice. The notes retain complete theoretical explanations and useful examples without reproducing the older full-application workload.
