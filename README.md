Amazon Price Tracker – Automated Web Scraping Pipeline
Python · BeautifulSoup · Requests · Data Pipeline · Automation

This project automates the extraction of Amazon product pricing, ratings, and review data and stores it as a continuously growing historical dataset.
It replicates a lightweight pricing intelligence system, similar to what retail, procurement, and e-commerce teams use to track competitor pricing and market changes.
Features

Daily automated scraping

Extracts:
Product title
Price (text + numeric)
Rating
Review count
Timestamp
Cleans & standardizes all fields
Appends data to a CSV dataset
Modular code (fetch → parse → clean → store)
Ready for Power BI dashboards & reporting
Optional price-drop alerts

📁 Project Structure
📦 amazon-price-tracker
│
├── Websc.ipynb                     # Development notebook
├── price_tracker.py                # (Optional) Clean script version
├── AmazonDataScraperDataset.csv    # Output dataset
└── README.md                       # Project documentation

#How It Works
1. Fetch HTML
Uses custom headers to safely request the Amazon product page.
2. Parse & Extract
BeautifulSoup extracts:
productTitle
price symbol, whole & fractional value
numeric price
rating (e.g., 4.6)
review count
3. Clean Data
Strip whitespace
Convert price to float
Standardize formatting
4. Store
Appends each daily run to a CSV for historical tracking.
5. Automate
A daily loop (time.sleep(86400)) triggers scraping every 24 hours.

#Code Example (Core Logic)
soup = BeautifulSoup(response.text, "html.parser")
title = soup.find(id="productTitle").get_text(strip=True)
symbol = soup.find("span", class_="a-price-symbol").get_text()
whole  = soup.find("span", class_="a-price-whole").get_text().replace(",", "")
frac   = soup.find("span", class_="a-price-fraction").get_text()
price_text = f"{symbol}{whole}{frac}"
price_num  = float(f"{whole}.{frac}")
reviews_text = soup.select_one("#acrCustomerReviewText").get_text()
reviews_count = int("".join(ch for ch in reviews_text if ch.isdigit()))
rating = float(soup.select_one("span.a-icon-alt").get_text().split()[0])

# Example Output
Date	Price	Price (Numeric)	Rating	Reviews
2024-01-10	£28.99	28.99	4.6	1932
2024-01-11	£29.49	29.49	4.6	1940

#Use cases:
Power BI/Looker dashboards
Price volatility tracking
Promotions vs. review surges
Pricing trend analysis

#Future Enhancements
Migrate storage from CSV → SQL/Postgres/BigQuery
Add Airflow / cron scheduling
Introduce retry logic + error logging
Use proxies + user-agent rotation
Track multiple products
Build a Power BI analytics dashboard
Add Slack/Email alerts for price drops

#Disclaimer
This project is for educational and portfolio purposes only.
Web scraping should respect site Terms of Service.
