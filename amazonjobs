import requests
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
import time

# Discord webhook URL (replace this with your actual webhook URL)
DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/your-webhook-url"

# Function to send a Discord notification
def send_discord_notification(message):
    data = {
        "content": message
    }
    response = requests.post(DISCORD_WEBHOOK_URL, json=data)
    if response.status_code == 204:
        print("Discord notification sent successfully.")
    else:
        print(f"Failed to send Discord notification: {response.status_code}")

# Set up Selenium WebDriver
def setup_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--start-maximized")  # Maximize window on start
    options.add_argument("--ignore-certificate-errors")  # Ignore SSL certificate errors
    options.add_argument("--disable-gpu")  # Disable GPU acceleration (optional)
    options.add_argument("--disable-dev-shm-usage")  # Overcome limited resource problems on Linux
    options.add_argument('--no-proxy-server')
    options.add_argument('--proxy-server="direct://"')
    options.add_argument('--proxy-bypass-list=*')
    options.add_argument("--allow-running-insecure-content")  # Allow insecure content
    options.add_argument("--disable-web-security")  # Disable web security if necessary
    options.add_argument("--disable-extensions")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")

    # Path to chromedriver (you can customize it if needed)
    service = Service('C:/Downloads/Compressed/chromedriver_win32o/chromedriver.exe')  # Replace with your actual path
    return webdriver.Chrome(service=service, options=options)

# Open Amazon job search page
def open_job_search_page(driver):
    driver.get("https://hiring.amazon.ca/app#/jobSearch")
    time.sleep(5)  # Wait for the page to load

# Function to automate job application process
def apply_to_jobs(driver, keywords):
    job_listings = driver.find_elements(By.CSS_SELECTOR, 'div.job-tile a.job-link')  # Update selector

    for job in job_listings:
        try:
            job_title = job.text

            # Check if any of the keywords are in the job title/description
            if any(keyword.lower() in job_title.lower() for keyword in keywords):
                job.click()
                time.sleep(3)

                while True:
                    try:
                        select_shift_button = driver.find_element(By.XPATH, '//*[@id="select-shift-button-id"]')  # Update XPath
                        select_shift_button.click()
                        time.sleep(0.5)

                        shift_option = driver.find_element(By.XPATH, '//*[@id="shift-option-id"]')  # Update XPath
                        shift_option.click()
                        time.sleep(0.5)

                        apply_button = driver.find_element(By.XPATH, '//*[@id="apply-button-id"]')  # Update XPath
                        apply_button.click()
                        time.sleep(1)

                        original_window = driver.current_window_handle
                        windows = driver.window_handles

                        for window in windows:
                            if window != original_window:
                                driver.switch_to.window(window)
                                break

                        time.sleep(1)

                        send_discord_notification(f"Shift picked successfully for job: {job_title}. Please fill in your application.")
                        print("Shift picked. Check Discord notification.")
                        break

                    except Exception:
                        time.sleep(1)
                        continue

        except Exception as e:
            print(f"Error applying to job: {e}")
            driver.back()
            time.sleep(2)

# Execute the script
if __name__ == "__main__":
    driver = setup_driver()
    open_job_search_page(driver)
    
    # Keywords for job filtering (add more if necessary)
    keywords = ['Brampton', 'Toronto', 'Mississauga']
    apply_to_jobs(driver, keywords)
