
# Automated Web Scraping of Dynamic Content with Selenium

This repository contains code and resources for automated web scraping of dynamic content using Selenium. The project demonstrates how to use Selenium to scrape data from web pages that load content dynamically.

## Repository Structure

```
Automated-Web-Scraping-of-Dynamic-Content-with-Selenium/
│
├── .gitattributes
├── Web Scraping Code File.ipynb
└── quotes.csv
```

- `.gitattributes`: Configuration file to ensure consistent handling of files across different operating systems.
- `Web Scraping Code File.ipynb`: Jupyter Notebook containing the code for web scraping using Selenium.
- `quotes.csv`: CSV file containing the scraped data.

## Introduction

This project focuses on using Selenium for web scraping to handle dynamic content that is loaded via JavaScript. The project involves the following key steps:

### Data Collection

- Set up Selenium WebDriver for automated browsing.
- Navigate to the target website and interact with web elements to load dynamic content.
- Extract the required data and save it to a CSV file.

## Key Technologies

- **Web Scraping**: Selenium, BeautifulSoup
- **Libraries**: Pandas, NumPy, Selenium
- **Data Storage**: CSV

## Getting Started

To get started with this project, follow the steps below:

### Prerequisites

Make sure you have the following installed:

- Python 3.x
- Jupyter Notebook
- Required Python libraries (listed in `requirements.txt`)
- Selenium WebDriver for your browser (e.g., ChromeDriver for Google Chrome)

### Installation

1. Clone this repository to your local machine:

```bash
git clone https://github.com/Harshraj1301/Automated-Web-Scraping-of-Dynamic-Content-with-Selenium.git
```

2. Navigate to the project directory:

```bash
cd Automated-Web-Scraping-of-Dynamic-Content-with-Selenium
```

3. Install the required Python libraries:

```bash
pip install -r requirements.txt
```

4. Download the appropriate Selenium WebDriver for your browser and ensure it is in your system's PATH.

### Usage

1. Open the Jupyter Notebook:

```bash
jupyter notebook "Web Scraping Code File.ipynb"
```

2. Follow the instructions in the notebook to run the code cells and scrape dynamic content using Selenium.

### Code Explanation

The notebook `Web Scraping Code File.ipynb` includes the following steps:

1. **Setting Up Selenium WebDriver**: Instructions for setting up Selenium WebDriver and navigating to the target website.
2. **Interacting with Web Elements**: Code for interacting with web elements to load dynamic content.
3. **Data Extraction**: Code for extracting the required data using Selenium and BeautifulSoup.
4. **Data Storage**: Saving the scraped data to a CSV file.

Here are the contents of the notebook:

# MGMT 590: Web Data Analytics | Homework #5

# Harshraj Vijaysinh Jadeja
## PUID - 36545027

## Code Cells

```python
pwd
```

```python
pip install selenium
```

```python
import csv
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Recording the start time
start_time = time.time()

# Initializing Chrome WebDriver
driver = webdriver.Chrome()

# Opening the website
driver.get("http://quotes.toscrape.com/js/")

# Initializing lists to store data
quotes_data = []

# Creating a WebDriverWait object with a timeout of 10 seconds
wait = WebDriverWait(driver, 10)

# Iterating through all pages
while True:
    # Finding all the quotes on the current page
    quotes = driver.find_elements(By.CLASS_NAME, "quote")
    
    # Extracting data for each quote on the current page
    for quote in quotes:
        quote_text = quote.find_element(By.CLASS_NAME, "text").text
        author = quote.find_element(By.CLASS_NAME, "author").text
        tags = "|".join(tag.text for tag in quote.find_elements(By.CLASS_NAME, "tag"))
        
        # Appending data to the list
        quotes_data.append([author, quote_text, tags])
    
    # Checking if there is a next page
    try:
        next_page = driver.find_element(By.XPATH, "//li[@class='next']/a")
        if 'disabled' in next_page.get_attribute("class"):
            print("Reached the last page.")
            break
    except:
        print("Next page button not found. Exiting.")
        break
    
    # Scrolling to the next page element
    print("Scrolling to the next page.")
    ActionChains(driver).move_to_element(next_page).perform()
    
    # Clicking the "Next" link
    print("Clicking the next page.")
    next_page.click()

# Closing the WebDriver
driver.quit()

# Recording the end time
end_time = time.time()

# Calculating the total run time
total_run_time = end_time - start_time
print(f"Total run time: {total_run_time} seconds")

# Saving the data to a CSV file
with open("quotes.csv", "w", newline="", encoding="utf-8") as csv_file:
    csv_writer = csv.writer(csv_file)
    
    # Writing header row
    csv_writer.writerow(["Author", "Quote", "Tags"])
    
    # Writing quote data
    csv_writer.writerows(quotes_data)

print("Quotes have been scraped and saved to quotes.csv.")

```

## Results

The notebook includes the results of the web scraping process, showcasing the extracted data saved in the `quotes.csv` file.

## Contributing

If you'd like to contribute to this project, please follow these steps:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature-branch-name`
3. Make your changes and commit them: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature-branch-name`
5. Submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- This project was created as part of an assignment by Harshraj Jadeja.
- Thanks to the open-source community for providing valuable resources and libraries for web scraping.

---

Feel free to modify this `README.md` file as per your specific requirements and project details.
