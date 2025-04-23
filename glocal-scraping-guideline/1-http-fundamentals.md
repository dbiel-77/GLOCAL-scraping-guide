# HTTP Fundamentals

This chapter introduces the core concepts of the Hypertext Transfer Protocol (HTTP), the foundation of data exchange on the web. You’ll learn how requests and responses are structured, which methods and status codes matter most for scraping, and best practices to keep your scraper polite and reliable.

---

## HTTP at a Glance

HTTP is a **stateless, request–response** protocol that runs over TCP/IP. When you scrape a government site, your scraper acts as an HTTP client, sending requests to the server and processing its responses.

- **Stateless**: Each request is independent; the server doesn’t retain memory of past interactions unless you manage cookies or tokens.
- **Request–Response Cycle**: A client issues a request, the server processes it, and returns a response.


## Anatomy of a URL

A typical URL looks like:

```
https://www.toronto.ca/city-government/council/members-of-council/
```

1. **Scheme**: `https` (secure HTTP) or `http` (unencrypted)  
2. **Host**: `toronto.ca`  
3. **Path**: `/city-government/council`  
4. **Query**: `?query=param` (key–value pairs)  
5. **Fragment**: `#section` (client‑side anchors)

### Benefits to decoding the URL
If you are scraping multiple pages - such as in our Toronto Council example, it is important to do a thorough check of the URL's that you plan to scrape. When I was writing the script here, I noticed that each of the 25 municipal councillors have a personal info page following the same format:

```
https://www.toronto.ca/city-government/council/members-of-council/councillor-ward-X/
```

Where X is a number from 1-25. Further analysis revealed that each of the councillor pages have an identical HTML structure - so, instead of writing 25 scripts for 25 pages, I could write one script that iterates through paths ../councillor-ward-X where x is 1 through 25:

```{code-block}
// Pseudocode
results = EMPTY LIST
BASE_URL    = 'https://www.toronto.ca/city-government/council/members-of-council/councillor-ward-{}'

FOR ward_number IN RANGE(1, 26):         // 1 through 25
    URL = FORMAT(BASE_URL, ward_number)
    record = scrape_ward(URL)
    IF record IS NOT None:
        APPEND record TO results

```

## Common HTTP Methods

| Method   | Description                               |
|:---------|:------------------------------------------|
| **GET**    | Retrieve data (safe and idempotent)      |
| **POST**   | Submit or modify data                    |
| **PUT**    | Replace resource entirely                |
| **PATCH**  | Partially update resource                |
| **DELETE** | Remove a resource                        |
| **HEAD**   | Same as GET but without response body    |
| **OPTIONS**| Discover supported methods on a resource |

In scraping, **GET** is king—most endpoints expose lists or pages via GET. Use **POST**/**PATCH** only if you’re interacting with an API that mandates it (e.g., form submissions).

---

## HTTPS & Security

Government sites almost always run over **HTTPS**. TLS encryption:

- **Encrypts** data in transit
- **Verifies** server identity via certificates

By default, `requests` validates certs. If you encounter a private or self-signed certificate, avoid disabling verification in production—better to add the CA bundle to your trust store.

---

### Further Reading

- [MDN: HTTP Basics](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)  
- [RFC 7230: HTTP/1.1 Message Syntax and Routing](https://tools.ietf.org/html/rfc7230)

