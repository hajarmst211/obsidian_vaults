
### The Project: "The Automated 'Wishlist' Arbitrage Bot"

**The Goal:**
You want to buy the "Top 50 Sci-Fi Books of All Time" (or Video Games / Movies), but you only want to buy them when their price drops below a certain amount on a specific e-commerce site. You also want to gather metadata (reviews, page count) to ensure it's the right edition.

---

### How to Combine the 5 Skills

Here is the step-by-step flow of how the code will run:

#### Step 1: The Seed (HTML Scraper)
**Task:** You need a list of things to track.
**The Code:** Use `BeautifulSoup` to go to a Wikipedia page (e.g., "Hugo Award-winning novels") or a simple blog list.
*   **Action:** Scrape the titles and authors of the top 50 entries.
*   **Output:** A Python list: `['Dune', 'Neuromancer', 'Hyperion'...]`.

#### Step 2: The Enrichment (API Consumer)
**Task:** The Wikipedia list is just names. You need precise data (ISBN, cover image, summary) to make sure you find the right product.
**The Code:** Iterate through your list and call the **OpenLibrary API** or **Google Books API**.
*   **Action:** Send the title `'Dune'`. Receive JSON back containing the specific ISBN (barcode number) and the page count.
*   **Output:** A dictionary: `{'title': 'Dune', 'ISBN': '9780441172719', 'pages': 412}`.

#### Step 3: The Spy (Headless Browser Bot)
**Task:** Now you need the price. The site you are checking (let's say a used book site or a tech site) has a search bar and loads prices dynamically using JavaScript.
**The Code:** Use `Selenium` or `Playwright`.
*   **Action:**
    1.  Launch a "headless" browser (no visible UI).
    2.  Navigate to the retailer's login page.
    3.  Input your username/password (to see "Member Only" prices).
    4.  Locate the search bar, type the **ISBN** from Step 2, and click search.
    5.  Scrape the price element (`<span class="price">$4.99</span>`).

#### Step 4: The Manners (Rate-Limiter)
**Task:** If you check 50 books in 2 seconds, the website will think you are a DDoS attack and block your IP address.
**The Code:** Implement a "Throttle" or "Backoff" function.
*   **Action:** Between every Selenium search, force the script to sleep for a random amount of time (e.g., between 5 and 10 seconds). If the site gives an error, wait 60 seconds before trying again.

#### Step 5: The History (Price Tracker)
**Task:** You need to know if today's price is good.
**The Code:** Save the data to a CSV file (or a SQLite database).
*   **Action:**
    1.  Read `price_history.csv`.
    2.  Append today's date, the book title, and the price found.
    3.  Compare today's price to the average price in the file.
    4.  **Bonus:** If `Current Price < Average Price`, print: "DEAL FOUND: BUY DUNE NOW!"

---

### The Problem Statement (Why does this exist?)

If you were pitching this project in a job interview, here is the problem you solved:

**The Problem: "Information Asymmetry and Fragmentation"**
1.  **Data Fragmentation:** The data I need is in three different places. The recommendation is on Wikipedia, the metadata is on an API, and the price is on a retailer site. A human cannot cross-reference these efficiently.
2.  **Dynamic Pricing:** Prices fluctuate daily based on algorithms. A human cannot manually check 50 different product pages every morning to catch a dip.
3.  **Inefficient UI:** Browsing e-commerce sites is slow. Automating the browser (Headless) creates a personalized data feed that bypasses the slow user interface.

### Technical Challenges You Will Face (The Learning Curve)

1.  **DOM Changes:** The retailer might change their website layout. Suddenly, your code looking for `div id="price"` will crash. You have to write robust code to handle errors.
2.  **Anti-Bot Detection:** Big sites (like Amazon) are very smart. They will detect Selenium and serve you a CAPTCHA. You will have to learn how to mimic human behavior (random mouse movements, random sleep times) or choose a friendlier target site for practice.
3.  **Data Matching:** Wikipedia might say "Harry Potter 1", but the API might return "Harry Potter Special Edition". You will have to write logic to handle "Fuzzy Matching" (string similarity).