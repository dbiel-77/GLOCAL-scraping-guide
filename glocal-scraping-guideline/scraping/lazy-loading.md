# Selenium and Lazy Loading

## What Is Lazy Loading?
Many modern sites defer loading images or other content until you scroll or interact. This “lazy loading” boosts performance but means a simple HTTP fetch won’t see everything—images may not exist in the raw HTML.

## How Selenium Works Around It
Selenium spins up a real (or headless) browser, lets JavaScript run, and simulates user actions like scrolling. Once the page is fully rendered, you grab the **complete DOM**—including any lazy-loaded elements—and parse it with BeautifulSoup.

## Selenium Setup in This Guide
We use Chrome in headless mode for speed and reliability:
- `--headless=new` to run without a UI  
- `--no-sandbox` and `--disable-dev-shm-usage` for container and CI compatibility  
- A simple `setup_driver()` helper wraps this config  

This setup is industry-standard for automated scraping, though you can swap in Firefox or other browsers if preferred.

---

## Example 1: Scraping without Selenium

Below is a slimmed down version of the Green Party image url scraper.

```python
import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin

def green_image_urls(url):
    container_selector = "article.gpc-post-card"
    image_selector = "img.attachment-post-thumbnail"

    response = requests.get(url)
    response.raise_for_status()

    soup = BeautifulSoup(response.text, "html.parser")
    containers = soup.select(container)
    img_urls = []

    for container in containers:
        img = container.select_one(image)
        if not img:
            continue # Skip if candidate does not have image

        # Since we are looking for image urls, not the images themselves - we may get lucky and
        # find the image source in the DOM, even though the image itself is unloaded
        src = img.get("data-src") or img.get("data-lazy-src") or img.get("src")
        if src:
            full_url = urljoin(url, src.strip())
            img_urls.append(full_url) # generates a complete url using the host and file path

    return img_urls

if __name__ == "__main__":
    target_url = "https://www.greenparty.ca/en/candidates/"
    image_urls = scrape_green_candidate_images_without_selenium(target_url)
    print(f"{len(image_urls)} image urls collected" )

```
Which outputs:
```bash
20 image urls collected
```
There are, however, far more than 20 Green Party candidates. To address this, the script was rewritten to include a basic Selenium setup: I configured the driver and wrote a function that tracks the total number of candidate cards on the page. After the first scroll, it finds 20 candidates—exactly what the initial scraper saw, yielding only 20 image URLs. A second scroll reveals 40 total candidates, a third yields 60, and so on. This continues until the nth scroll, where 236 candidates are found; when one more scroll still returns 236, scraping can commence:

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from bs4 import BeautifulSoup
from urllib.parse import urljoin

def setup_driver(headless=True):
    # This is a common configuration, and will almost always work
    # refer to the selenium docs for more information
    # https://www.selenium.dev/documentation/webdriver/getting_started/first_script/
    options = Options()
    if headless:
        options.add_argument("--headless=new")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    service = Service(ChromeDriverManager().install())
    return webdriver.Chrome(service=service, options=options)

# try different max_tries limits to see if the quantity of data changes
# if increasing it leads to more data, then keep testing until you find the 'magic' number
# For example, you can set max_tries to 100000000, and _ to i, print i at curr_count == prev_count
def scroll_until_no_new_candidates(driver, selector, max_tries=30, pause=2):
    prev_count = 0 # initialize the count
    for _ in range(max_tries):
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(pause)
        elements = driver.find_elements(By.CSS_SELECTOR, selector) # finding elements with selenium
        curr_count = len(elements)
        if curr_count == prev_count:
            break
        prev_count = curr_count

def green_candidate_images(url):
    container_selector = "article.gpc-post-card"
    image_selector = "img.attachment-post-thumbnail"

    driver = setup_driver() # setup headless browser
    driver.get(url)
    time.sleep(3) # allow page to load before scrolling

    scroll_until_no_new_candidates(driver, container_selector)

    soup = BeautifulSoup(driver.page_source, "html.parser") # scrape the loaded data
    driver.quit() # close headless browser

    containers = soup.select(container_selector)
    print(f"Found {len(containers)} candidate containers after scrolling.")

    img_urls = []

    for container in containers:
        img = container.select_one(image_selector)
        if not img:
            continue
        src = img.get("data-src") or img.get("data-lazy-src") or img.get("src")
        if src:
            full_url = urljoin(url, src.strip())
            img_urls.append(full_url)

    return img_urls

if __name__ == "__main__":
    target_url = "https://www.greenparty.ca/en/candidates/"
    image_urls = green_candidate_images(target_url)
    print(f"{len(image_urls)} image urls collected")
```

Yielding a much more believable output of
```bash
Found 236 candidate containers after scrolling.
209 image urls collected
```