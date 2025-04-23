# HTML Structure for Scraping

This page dives into the anatomy of an HTML document - the skeleton of every page on the web, and how to explore it using your browser's developer tools

---

## The HTML Skeleton

Every HTML page follows a structure that can be summarized as the following:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Metadata, CSS links, scripts -->
  <meta charset="UTF-8">
  <title>Page Title</title>
</head>
<body>
  <!-- Visible content: headers, nav, main, footer -->
  <header>…</header>
  <nav>…</nav>
  <main>…</main> <!-- Where most of your data will be found -->
  <aside>…</aside>
  <footer>…</footer>
  <!-- JavaScript at end of body -->
  <script src="..."></script>
</body>
</html>
```

Key sections:

- `<!DOCTYPE html>` declares HTML5
- `<head>` holds metadata, CSS links, fonts, and scripts needed before rendering.
- `<body>` contains the actual page structure you’ll scrape: headings, paragraphs, tables, lists, images, and interactive components.

---

## Common Structural Elements

| Element   | Purpose                                  |
|-----------|------------------------------------------|
| `<header>` | Site header—logo, primary navigation    |
| `<nav>`    | Navigation links or breadcrumb trails   |
| `<main>`   | Main content area (articles, forms)     |
| `<section>`| Thematic grouping of content            |
| `<aside>`  | Sidebar—related links or ads            |
| `<footer>` | Footer—contact info, legal links        |

Here is a simplified excerpt from the Toronto Councillor page for ward 1:
```html
<main>
  <h1 id="page-header--title">Councillor Vincent Crisanti</h1>
    <div id="page-content" class="col-md-8 col-lg-9">
        <h2>Etobicoke North</h2>
        <p>
            <img fetchpriority="high" decoding="async" class="alignnone wp-image-744932" src="https://www.toronto.ca/wp-content/uploads/2023/01/968e-2216710W01CouncillorVincentCrisanti2-5 0x625.jpg" alt="Portrait of Councillor Vincent Crisanti" width="250" height="313">
        </p>
  </section>
</main>
```
Using BeautifulSoup, you could extract the name, riding and photo url as follows:

```python
name = soup.find('h1', id='page-header--title').get_text(strip=True)
# Variable 'Name' = the text contents of an h1 element with the id 'page-header--title', with whitespace stripped
```

Since the name has an ID, we can be certain that if we are getting exactly what we are looking for when we scrape for a name. But what happens if there are multiple image, or h2 elements on a page?

First, identify the container that the element is stored within - in this case, a div with the id 'page-content'. Assuming that there are no other images or h2 elements in this container, we can proceed to scrape those elements explicitly within that container:

```python
page_content = soup.find('div', id='page-content')
riding = page_content.find('h2').get_text(strip=true)
photo_url = page_content.find('img', src=True)['src']
```

```
Name:      Councillor Vincent Crisanti
Riding:    Etobicoke North
Photo URL: https://www.toronto.ca/wp-content/uploads/2023/01/968e-2216710W01CouncillorVincentCrisanti2-50x625.jpg
```

In practice, you will come across all kinds of unique cases - far too many to document in a guide. With some problem-solving, anything can be achieved - scraping some data off of a website is nowhere near as straightforward as it may seem - each unique page presents a unique problem that requires a unique approach.

---

## Dynamic Components

Many pages include interactive widgets or accordions:

```html
<div class="accordion">
  <button class="toggle" aria-expanded="false" data-target="#profile">Profile</button>
  <div id="profile" class="panel">…</div>
</div>
```

For scraping, you often need the expanded content, so check if the HTML is present in the source (`View Page Source`) - if it is, then you can proceed as usual.

If the content is loaded dynamically, for example - an image that only loads once you scroll down to it, then you will need to run Selenium, which creates a virtual chromium browser and scrapes from there. That process is explained in #lazyloaded.

---

## Inspecting with Browser DevTools

1. **Open DevTools**: Press `F12` (Windows) or `Cmd+Option+I` (Mac)
2. **Elements panel**: Browse the live DOM tree—hover to highlight elements on the page.
3. **Search panel**: Press `Ctrl+F` to find by CSS selector, text, or XPath.
4. **Copy selector**: Right‑click an element → **Copy** → **Copy selector**—use this in your scraper to target elements.
5. **Network panel**: Monitor `XHR` or `Fetch` requests to discover JSON endpoints powering dynamic content.

---

## Setting up a scraper configuration
Be sure to make note of the elements that you want to scrape and keep them separate from the rest of the script. When I was scraping federal election candidates for example, I created one unique script per party, with a configuration at the very top. The configuration contained and organized selecters and definitions in a key pair format - the script will call the value when needed, as opposed to hardcoding the selector itself (multiple times). 

```
"Conservative": {
    "url": "https://www.conservative.ca/candidates/",
    "selectors": {
        "container": "div.card-wrapper",
        "name": "h3.name-header",
        "riding_title": "p.riding-title",
        "social_container": "ul.candidate-social-nav li",
        "detail_link": "a.button",
        "affiliation": "Conservative"
```
In the event of a major page redesign, you will only need to update the config you created at the top.

```{tip}
Use Robust Selectors

- Prefer **ID** selectors (`#page-header--title`) when available—they’re unique.
- Use **class** or **attribute** selectors when no ID exists: `.staff-list li`, `div[role="main"]`.
- Avoid brittle absolute paths; instead, build relative selectors: `section#staff ul li` over `/html/body/main/section[2]/ul/li[1]`.

```python
# Example CSS selector in BeautifulSoup
staff_items = soup.select('section#staff ul li')
for item in staff_items:
    name, email = item.get_text(), item.find('a').get('href')
```

```
## Further Reading
- [MDN: HTML Introduction](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [HTML Living Standard – WHATWG](https://html.spec.whatwg.org/)
- [MDN: HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

