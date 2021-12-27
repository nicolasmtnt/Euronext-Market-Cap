#  Market Capitalisation Data Visualization
## Country Ranking

By using data from companiesmarketcap.com, we can look at distribution of countries by their publicly listed companies MarketCap.
The result show that that total value of US listed companies represent more than half of total world market capitalization

country|marketcap|%
---|---|---
United States|52299|53.58
China|7297|7.48
Japan|4478|4.59
India|3026|3.1
France|3005|3.08
Germany|2518|2.58
United Kingdom|2517|2.58
Canada|2494|2.56
Saudi Arabia|2270|2.33
Switzerland|2257|2.31
Netherlands|1535|1.57
Taiwan|1440|1.48
Australia|1328|1.36
South Korea|1259|1.29

(data taken on December 27, 2021)
[Complet List](country_by_marketcap.ipynb)


## Euronext-Market-Cap
### Simple data visualisation of largest French listed companies on Euronext Paris by Market Cap

In this project, we use Python-based tools (Re, Pandas, BeautifulSoup) to display the largest market capitalizations on Euronext market.
![Paris 60 largest Cap](piechart40.png)
(Screening performed on November 26, 2021)

## Protocol
First we import data from Euronext website and then parse with __*pandas*__ library.
```
df= pd.read_excel("dataParis.xlsx")
df.drop([0,1,2], inplace=True)
df.reset_index(drop=True, inplace=True)
```

#### Then we iterate threw each row and use Euronext API to get Market Capitalization with __*BeautifulSoup*__.
Necessary import :
```
import pandas as pd
pd.options.display.max_rows = 50
import requests
from bs4 import BeautifulSoup
import re
import matplotlib.pyplot as plt
```

Firstly, we create a function which use Euronext API to get Market Cap of each company by ISIN
```
def getMarketCap(ISIN):
    url = f"https://live.euronext.com/fr/intraday_chart/getDetailedQuoteAjax/{ISIN}-XPAR/full"
    response = requests.get(url)
    if(response.ok):
        soup = BeautifulSoup(response.text)
        try:
            capitalization = soup.find("tbody").findAll('tr')[11].find(class_="font-weight-medium").text
            return capitalization
        except IndexError:
            return 0
```

Then we convert from String to float the result of the previous function
```
def convertToInt(ISIN):
    if(type(ISIN)==str):
        if("Md" in ISIN):
            return (float(ISIN.replace("Md","").replace(",",".")))
        if("Md" in ISIN):
            return 0
```

Data visualization code
```
def pieByPercent(i,size=150):
    plt.rcParams['figure.dpi'] = size
    plt.rcParams['savefig.dpi'] = size
    name = df['Name'].to_list()
    marketCap = df['MarketCap'].to_list()
    plt.pie(marketCap[0:i], labels =name[0:i], wedgeprops={'edgecolor':'black', 'linewidth': 0.5}, shadow=True, autopct='%1.1f%%', textprops={'fontsize': 1500/size})
    plt.title(f"Largest {i} french companies listed on Euronext Paris")
    plt.savefig(f"piechart{i}")

```

This process is performed only on stocks valued at more than 1 billion euros (using __*re*__)

![result table](result.csv)


![Amsterdam 25 largest Cap](amsterdam25.png)

note : Some companies like __*Shell*__ or __*Unilever*__ will not appear because their financial id (ISIN) is registered in the UK
