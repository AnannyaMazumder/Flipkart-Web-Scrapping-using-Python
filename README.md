# **Flipkart Web Scrapping using Python**


## Introduction
In the era of e-commerce dominance, having access to real-time product information is crucial for making informed decisions. Web scraping, the automated extraction of data from websites, provides a powerful means of gathering up-to-date information from online platforms. This project harnesses the capabilities of Python, along with the BeautifulSoup and Requests libraries, to perform real-time web scraping from Flipkart, one of the leading e-commerce websites in India.



## Python Notebook Run with Outputs

```python
# import libraries 

from bs4 import BeautifulSoup
import requests
import time
import datetime

import smtplib
```


```python
# Connect to Website and pull in data

URL ="https://www.flipkart.com/souled-store-printed-women-round-neck-maroon-t-shirt/p/itm1acb195b6f8f1?pid=TSHGG68TX4SNVECR&lid=LSTTSHGG68TX4SNVECRPG00XN&marketplace=FLIPKART&q=harry+potter+tshirt+for+women&store=clo%2Fash%2Fank%2Floi&srno=s_1_22&otracker=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&otracker1=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&fm=search-autosuggest&iid=abe34160-a1cf-46ef-937d-7d320c2ba776.TSHGG68TX4SNVECR.SEARCH&ppt=sp&ppn=sp&ssid=7lv02ort740000001712772560434&qH=d3dcbb6c1ad0b061"
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

page = requests.get(URL, headers=headers)

soup1 = BeautifulSoup(page.content, "html.parser")

soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

title = soup2.find(class_ ="B_NuCI" ).get_text()

price = soup2.find(class_ ="_30jeq3 _16Jk6d").get_text()


print (title)
print (price)


```

    
                Harry Potter Women Printed Round Neck Pure Cotton Maroon T-Shirt
               
    
                 ₹599
                
    


```python
# Clean up the data a little bit

price = price.strip()[1:]
title = title.strip()

print(title)
print(price)
```

    Harry Potter Women Printed Round Neck Pure Cotton Maroon T-Shirt
    599
    


```python
# Create a Timestamp for your output to track when data was collected

import datetime

today = datetime.date.today()

print(today)
```

    2024-04-13
    


```python
# Create CSV and write headers and data into the file

import csv 

header = ['Title', 'Price', 'Date']
data = [title, price, today]

with open('FlipkartWebScraperDataset.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)
    
```


```python
import pandas as pd

df = pd.read_csv(r'C:\Users\anann\FlipkartWebScraperDataset.csv')
 
print(df)
```

                                                   Title  Price        Date
    0  Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    


```python
import csv

data = [title, price,today]

#Now we are appending data to the csv

with open('FlipkartWebScraperDataset.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)

```


```python
#Combine all of the above code into one function


def check_price():
    URL ="https://www.flipkart.com/souled-store-printed-women-round-neck-maroon-t-shirt/p/itm1acb195b6f8f1?pid=TSHGG68TX4SNVECR&lid=LSTTSHGG68TX4SNVECRPG00XN&marketplace=FLIPKART&q=harry+potter+tshirt+for+women&store=clo%2Fash%2Fank%2Floi&srno=s_1_22&otracker=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&otracker1=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&fm=search-autosuggest&iid=abe34160-a1cf-46ef-937d-7d320c2ba776.TSHGG68TX4SNVECR.SEARCH&ppt=sp&ppn=sp&ssid=7lv02ort740000001712772560434&qH=d3dcbb6c1ad0b061"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

    page = requests.get(URL, headers=headers)

    soup1 = BeautifulSoup(page.content, "html.parser")

    soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

    title = soup2.find(class_ ="B_NuCI" ).get_text()

    price = soup2.find(class_ ="_30jeq3 _16Jk6d").get_text()

    price = price.strip()[1:]
    title = title.strip()


    import datetime

    today = datetime.date.today()
    
    import csv 

    header = ['Title', 'Price', 'Date']
    data = [title, price, today]

    with open('FlipkartWebScraperDataset.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)

    if(int(price) < 500) :
        send_mail()
```


```python
# Runs check_price after a set time and inputs data into your CSV

while(True):
    check_price()
    time.sleep(5)
```


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    Cell In[9], line 5
          3 while(True):
          4     check_price()
    ----> 5     time.sleep(5)
    

    KeyboardInterrupt: 



```python
import pandas as pd

df = pd.read_csv(r'C:\Users\anann\FlipkartWebScraperDataset.csv')
 
print(df)
```

                                                    Title  Price        Date
    0   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    1   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    2   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    3   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    4   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    5   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    6   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    7   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    8   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    9   Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    10  Harry Potter Women Printed Round Neck Pure Cot...    599  2024-04-13
    


```python
# If uou want to try sending yourself an email (just for fun) when a price hits below a certain level you can try it
# out with this script

def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com',465)
    server.ehlo()
    #server.starttls()
    server.ehlo()
    server.login('anannyam@gmail.com','xxxxxxxxxxxxxx')
    
    subject = "The Shirt you want is below ₹500! Now is your chance to buy!"
    body = "Anannya, This is the moment we have been waiting for. Now is your chance to pick up the shirt of your dreams. Don't mess it up! Link here: 'https://www.flipkart.com/souled-store-printed-women-round-neck-maroon-t-shirt/p/itm1acb195b6f8f1?pid=TSHGG68TX4SNVECR&lid=LSTTSHGG68TX4SNVECRPG00XN&marketplace=FLIPKART&q=harry+potter+tshirt+for+women&store=clo%2Fash%2Fank%2Floi&srno=s_1_22&otracker=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&otracker1=AS_QueryStore_OrganicAutoSuggest_1_19_na_na_na&fm=search-autosuggest&iid=abe34160-a1cf-46ef-937d-7d320c2ba776.TSHGG68TX4SNVECR.SEARCH&ppt=sp&ppn=sp&ssid=7lv02ort740000001712772560434&qH=d3dcbb6c1ad0b061%22' 
   
    msg = f"Subject: {subject}\n\n{body}"
 server.sendmail(
        'anannyam9@gmail.com',
        msg
```


  ##  Project Overview:
The project focuses on developing a Python script that extracts product data in real-time from Flipkart's website using web scraping techniques. By leveraging BeautifulSoup for parsing HTML content and Requests for making HTTP requests, the script enables users to retrieve dynamic product information directly from Flipkart's product pages.

## Key Features:
1. **Dynamic Data Retrieval:** The script sends HTTP requests to Flipkart's servers and parses the HTML content of product pages in real-time. This ensures that users obtain the latest product information available on the website.
2. **Flexible Data Extraction:** Using BeautifulSoup's powerful parsing capabilities, users can specify the data elements they wish to extract from Flipkart's product pages. This includes product name, price.
3. **Efficient HTTP Handling:** The use of the Requests library allows for efficient handling of HTTP requests and responses, ensuring reliable communication with Flipkart's servers during the scraping process.
4. **Structured Data Output:** The extracted product data is formatted into a structured format, such as CSV or JSON, for easy analysis and integration with other applications or databases. This facilitates further processing and visualization of the scraped data.

## Conclusion
The real-time web scraping project from Flipkart using BeautifulSoup and Requests offers a valuable tool for extracting up-to-date product information from the e-commerce platform. By automating the data extraction process, users can stay informed about product availability, pricing trends, and customer feedback in real-time. With its flexibility, efficiency, and structured output, the project serves as a versatile solution for various applications in e-commerce analytics, market research, and competitive intelligence.


   
