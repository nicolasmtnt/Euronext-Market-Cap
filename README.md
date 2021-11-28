# Euronext-Market-Cap
### Simple data visualisation of largest French listed companies on Euronext Paris by Market Cap

![Paris 60 largest Cap](piechart40.png)
In this project, we use Python-based tools (Re, Pandas, BeautifulSoup) to display the largest market capitalizations on Euronext market.
![Paris 60 largest Cap](piechart100.png)
(Screening performed on November 28, 2021)

## Protocol
First we import data from Euronext website and then parse with __*pandas*__ library.
Then we iterate threw each row and use Euronext API to get Market Capitalization with __*BeautifulSoup*__.
This process is performed only on stocks valued at more than 1 billion euros (using __*re*__)

##

