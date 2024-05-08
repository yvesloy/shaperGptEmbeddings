## Global Shaper Data Scraping & Embedding

### Overview
This project extracts content from global shaper web pages, processes it, and stores the data along with generated embeddings into a PostgreSQL database. It uses various Python libraries to handle web scraping, text splitting, and database interactions.

### Installation

Ensure Python 3.10 is installed on your machine. Use Pipenv to set up the environment and install dependencies:

```
pip install pipenv
pipenv install
```

### Configuration
Populate the `.env.template` with your PostgreSQL and OpenAI credentials and rename it to `.env`:

### Database Setup
The project uses PostgreSQL with an extension for handling vectors. Ensure your database supports the vector extension.

### Hosting
Database hosting is managed via Vercel, supporting seamless integration with other services on the same platform.

### Data Extraction

The data extraction process is primarily handled through the Page class, which encapsulates methods for navigating to, parsing, and extracting content from a URL. Here’s a step-by-step breakdown of how the process works:

1. Page Initialization
Each page is represented by an instance of the Page class, initialized with a specific URL. This object stores various properties such as the page's URL, title, raw HTML source, and the textual content.

2. Parsing the Page
The parse_page method of the Page class uses Selenium with a headless Chrome driver to navigate to the URL and load the web page. This method captures the full HTML source and extracts the textual content from a specified element class, typically where the main content of the page resides. The title of the page is also extracted from the HTML <title> tag if available.
Using Selenium allows the script to interact with JavaScript-rendered pages, which is crucial for extracting data from dynamically generated web pages. The headless browser can execute JavaScript and render the page as it would appear in a standard web browser.

3. Extracting Adobe Links
After parsing the main page, the extract_adobe_links method searches through the HTML to find links that contain specific Adobe-related domains (this is the only information we need at the moment).

4. Processing Additional Pages
Once the main page and relevant Adobe links are extracted, the script processes these URLs in a similar manner, parsing each page and extracting the content. This allows the script to navigate through a network of connected pages automatically.

5. Text Splitting
The extracted text from each page is processed using the TokenTextSplitter from the langchain-text-splitters library. This method splits the text into smaller chunks based on token count.
