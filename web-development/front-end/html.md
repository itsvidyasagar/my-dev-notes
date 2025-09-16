# 🌐 HTML Notes

This file contains essential HTML tags and concepts for reference.  
Use it as a quick guide while learning or building web pages.

## 1. Document Structure & Metadata

| HTML Tag | Description |
|----------|-------------|
| `<!DOCTYPE html>` | Declares the document type and HTML version. |
| `<html lang="en">` | Root HTML element; `lang` specifies the language. |
| `<head>` | Contains metadata, links to stylesheets/scripts, and the `<title>`. |
| `<meta charset="UTF-8">` | Sets character encoding. |
| `<meta name="viewport" content="width=device-width, initial-scale=1.0">` | Makes the page responsive on mobile devices. |
| `<title>` | Title of the HTML document shown in the browser tab. |
| `<link rel="stylesheet" href="style.css">` | Links an external CSS file. |
| `<script src="script.js"></script>` | Links an external JavaScript file. |
| `<body>` | Contains all content displayed in the browser. |
| `<!-- comment -->` | HTML comment; not displayed in the browser. |

## 2. Headings & Text Formatting

| HTML Tag | Description |
|----------|-------------|
| `<h1> … <h6>` | Headings from most important (`<h1>`) to least important (`<h6>`). |
| `<p>` | Paragraph element. |
| `<br>` | Line break. |
| `<hr>` | Horizontal rule. |
| `<b>` | Bold text (non-semantic). |
| `<strong>` | Emphasized bold text (semantic). |
| `<i>` | Italic text (non-semantic). |
| `<em>` | Emphasized italic text (semantic). |
| `<u>` | Underlined text. |
| `<mark>` | Highlighted text. |
| `<sub>` | Subscript text (e.g., CO₂). |
| `<sup>` | Superscript text (e.g., n²). |
| `<blockquote>` | Quoted section of text. |
| `<code>` | Inline code. |
| `<pre>` | Preformatted text block. |

## 3. Links, Images & Media

| HTML Tag | Description |
|----------|-------------|
| `<a href="https://www.google.com">Google</a>` | External link (absolute URL). |
| `<a href="/home.html">HomePage</a>` | Internal link (relative URL). |
| `<img src="https://www.example.com/image.jpg" alt="Image not found">` | Displays an image from an external source. |
| `<img src="/images/23.jpg" alt="Image not found">` | Displays an image from the same project/repo. |
| `<video src="/video/23.mp4" controls></video>` | Video element (local or external). |
| `<audio src="audio.mp3" controls></audio>` | Audio element (local or external). |
| `<iframe src="https://example.com"></iframe>` | Embeds another webpage (e.g., YouTube video). |

## 4. Lists & Tables

| HTML Tag | Description |
|----------|-------------|
| `<ul>` | Unordered list. |
| `<ol>` | Ordered list. |
| `<li>` | List item. |
| `<table>` | Table element; use `<thead>` and `<tbody>` for SEO and accessibility. |
| `<thead>` | Table header section (contains `<tr>` with `<th>`). |
| `<tbody>` | Table body section (contains `<tr>` with `<td>`). |
| `<tr>` | Table row. |
| `<th>` | Table header cell. |
| `<td>` | Table data cell. |

## 5. Forms & Inputs

| HTML Tag | Description |
|----------|-------------|
| `<form>` | Form element to collect user input. |
| `<label for="id1"><input type="text" id="id1"></label>` | Labels an input element for accessibility. |
| `<input type="text" name="username" placeholder="Enter text">` | Text input. |
| `<input type="password" name="password" placeholder="Enter password">` | Password input. |
| `<input type="email" name="email">` | Email input. |
| `<input type="number" name="age" min="0" max="120">` | Number input. |
| `<input type="date" name="dob">` | Date input. |
| `<input type="color" name="color">` | Color picker input. |
| `<input type="file" name="file">` | File upload input. |
| `<input type="radio" name="gender" value="male">` | Radio button; only one per `name` can be selected. |
| `<input type="checkbox" name="question1" value="option1">` | Checkbox; multiple can be selected per `name`. |
| `<select name="city"><option value="delhi">Delhi</option></select>` | Dropdown list. |
| `<textarea name="message"></textarea>` | Multi-line text input. |
| `<input type="button" value="Click me">` | Button; triggers JavaScript actions. |
| `<input type="submit" value="Submit">` | Submits the form. |

## 6. Semantic Layout & Miscellaneous

| HTML Tag | Description |
|----------|-------------|
| `<header>` | Header section of the webpage. |
| `<nav>` | Navigation block for links. |
| `<main>` | Main content area; can include `<section>`, `<article>`, `<aside>`. |
| `<section>` | Defines a thematic section (e.g., Experience, Projects). |
| `<article>` | Contains a self-contained piece of content. |
| `<aside>` | Sidebar content; often unrelated to main content. |
| `<footer>` | Footer section of the webpage. |
| `<div>` | Block-level non-semantic container. |
| `<span>` | Inline non-semantic container. |
| `class="item-class"` | Groups multiple elements for styling. |
| `id="unique-id"` | Identifies a single, unique element. |
| `<details>` & `<summary>` | Collapsible sections. |
| `<link rel="icon" href="favicon.ico">` | Favicon for the browser tab. |

## 7. Accessibility Notes

- **Semantic HTML:** Use elements like `<header>`, `<main>`, `<section>`, `<article>`, `<footer>` to clearly describe the purpose of content.  
  - Helps **browsers, search engines, and screen readers** understand the page structure.  
  - **Non-semantic elements** like `<div>` and `<span>` do not convey meaning and are mainly for styling or grouping.
- **Images:** Always add `alt` attributes to describe the content of images.
- **Forms:** Properly label form elements using `<label>` with `for` attributes to improve usability.
- **ARIA:** Use `aria-*` attributes when needed to enhance accessibility for assistive technologies.
