## Description of the Datasets in Use

We will use three types of datasets in this hands-on training.

| Data Source | Dataset | File Name |
| --- | --- | --- |
| Kaggle | Credit card fraud detection | Creditcard.csv |
| Open Weather | List of city latitudes and longitudes | Citylist.csv |
| ISO3166-1 | List of country codes | Countrylist.csv |

### Description of the Credit Card Fraud Detection Dataset

The credit card fraud detection dataset uses data obtained by converting the data from transactions made by European cardholders in September 2013 that was converted by principal component analysis (PCA). It is considered inappropriate to provide the original features and more background information about the data, due to personal information protection issues associated with GDPR that came into effect in May 2017.

(Source: [https://kaggle.com/mlg-ulb/creditcardfraud](https://kaggle.com/mlg-ulb/creditcardfraud))

| No | Column ID | Column Name | Description |
| -- | --------- | ----------- | ----------- |
| 1 | Time | Time | Contains the number of seconds elapsed between each transaction and the first transaction in the dataset. |
| 2 - 29 | V1, V2, ... V28 | PCA conversion results | Data obtained by converting the data in the column that affects the detection of fraud from credit card transaction data using principal component analysis (PCA). |
| 30 | Amount | Amount | Credit card transaction amount |
| 31 | Class | Fraudulent/Non-fraudulent | A column that determines whether a transaction is fraudulent or non-fraudulent, as defined for a fraud detection machine learning model. (0=Non-fraudulent, 1=Fraudulent) |
| 32 | Id | City ID | A column which associates credit card transactions with city IDs in the city latitude and longitude list.  For this hands-on training, columns have been added and data has been processed/edited to include these city IDs. |

### Description of the City Latitude and Longitude List Dataset

Use CSV data from the city latitude and longitude list that is published in OpenWeather, a company that publishes the latitude and longitude data of cities around the world.

(Source: [https://openweathermap.org](https://openweathermap.org))

| No | Column ID | Column Name | Description |
| -- | --------- | ----------- | ----------- |
| 1 | Id | City ID | A surrogate key representing the city |
| 2 | Name | City name | The city name |
| 3 | State | State | The state name, if applicable |
| 4 | alpha2 | Two-character country code | ISO3166-1 alpha-2 (two-digit country code) |
| 5 | Lon | Longitude | Value showing the longitude of the city |
| 6 | Lat | Latitude | Value showing the latitude of the city |

### Description of the Country Code List Dataset

CSV data created from a list of international standards for codes showing ISO3166-1 country names and administrative districts and territories.

(Source: [https://jp.wikipedia.org/wiki/ISO_3166-1](https://jp.wikipedia.org/wiki/ISO_3166-1))

| No | Column ID | Column Name | Description |
| -- | --------- | ----------- | ----------- |
| 1 | companyjp | Country Name (Japanese) | Contains the Japanese-language name of the country |
| 2 | companyen | Country Name (English) | Contains the English-language name of the country |
| 3 | numeric | Numeric Code | ISO3166-1 numeric code |
| 4 | alpha3 | Three-character country code | ISO3166-1 alpha-3 (three-character country code) |
| 5 | alpha2 | Two-character country code | ISO3166-1 alpha-2 (two-character country code) |
| 6 | location | Location | Contains the Japanese-language location |
| 7 | subdivision | Subdivision Code | ISO 3166-2 subdivision code |

