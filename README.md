# BMDC Scraper

This project is designed to scrape data from **BMDC Doctor's Profile Web Service**, handle CAPTCHA authentication, and process the data into a `csv` file. The detailed documentation of the functions involved in this project is detailed below.

### Function Definitions

#### `open_selenium_browser(browser_name: str, headless: bool)`

This function opens a Selenium WebDriver instance with the specified browser name and headless mode. Supported browser names are "chrome", "edge", "firefox", and "safari". If headless mode is set to True, the browser will be run in the background without any visible UI.

#### `go_to_page_with_selenium(driver, url: str="https://verify.bmdc.org.bd/") -> tuple[WebDriver, str]`

This function navigates the WebDriver to the specified URL (default is 'https://verify.bmdc.org.bd/') and waits until the page is fully loaded. It then retrieves the HTML content of the page and returns a tuple containing the WebDriver instance and the page source.

#### `get_captcha_image(page_source)`

This function extracts the CAPTCHA image from the page source by finding the appropriate HTML element and reading its 'src' attribute. It then sends a GET request to this URL to download the image, converts it into a numpy array, and returns it.

#### `process_image(img)`

This function processes the CAPTCHA image to make it suitable for OCR. It inverts the colors, erodes the image, and then creates a new blank image with padding around the original. The processed image is then returned.

#### `solve_captcha(img)`

This function uses the easyOCR library to recognize the text in the CAPTCHA image and returns the recognized text.

#### `submit_form_selenium(driver,  doc_id, captcha_solution,)`

This function fills the form on the webpage with the provided document ID and CAPTCHA solution. It then clicks the submit button and returns the WebDriver and the current URL.

#### `get_doctor_dict_selenium(driver)`

This function extracts doctor information from the webpage after the form has been submitted. It finds various elements on the page by their XPath, extracts their text content, and stores it in a dictionary, which is then returned.

#### `single_doc_entry(id, browser_name, headless)`

This function is the main workflow that combines all the previously defined functions. It opens a Selenium browser, navigates to the webpage, repeatedly attempts to solve the CAPTCHA and submit the form until successful, then extracts the doctor information and returns it.

#### `main_multiprocess(doc_id_start, doc_id_end, browser_name, headless, workers=4)`

This is the main entry point of the script. It takes a range of document IDs, a browser name, and a headless mode as input, and processes each document ID in a separate process using Python's concurrent.futures library. The results are combined into a pandas DataFrame and returned.

### How to Run This Script

This script can be run by calling the `main_multiprocess` function with the desired arguments, e.g.,

```python
df = main_multiprocess(doc_id_start=1000, doc_id_end=2000, browser_name='chrome', headless=True, workers=4)
```

This will scrape data for document IDs from 1000 to 2000 using 4 concurrent processes, and store the resulting data in a pandas DataFrame.


# Installation
First, clone the repository or download the script. Then, install the required dependencies using pip:

```bash
pip install -r requirements.txt
```

# Usage
You can run the script using the following command in your terminal:
#### Headless Mode
```bash
python main.py --browser firefox --start 1 --end 50
```
By default, the script runs browser in headless mode. To run the browser in normal mode, use the `--headless` flag:

#### Normal Mode
```bash
python main.py --browser firefox --headless --start 1 --end 50
```
