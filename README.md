# Octave-Platform: Stock Factor Analysis

This is a Python application built using the Streamlit library that allows users to analyze stock factors for a given region and stock ticker symbol.

## Requirements

This application requires the following Python libraries to be installed:

-   streamlit
-   pandas
-   matplotlib
-   seaborn
-   statsmodels

## Usage

To use the application, simply run `app.py` and the application will launch in your browser.

Once launched, the user is presented with a dropdown menu to select a region and a text input to enter a stock ticker symbol. Once the user selects the region and enters a valid stock ticker symbol and clicks the "Analyze" button, the following actions take place:

1.  The program retrieves factor data for the selected region from the following website: "[https://mba.tuck.dartmouth.edu/pages/faculty/ken.french](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french)"
2.  The program merges the retrieved factor data with the momentum data.
3.  The program downloads historical prices for the entered stock ticker symbol and merges it with the merged factor and momentum data.
4.  The program runs a regression on the merged data using the factors as independent variables and calculates the Alpha, Beta_Mkt, Beta_SMB, Beta_HML, and Rf values.
5.  The program displays the calculated values along with a summary of the regression.
6.  The program generates four plots: Factor Returns Over Time, Momentum Returns Over Time, Historical Stock Returns Over Time, and Fitted vs. Actual Stock Returns.

## Customization

If you want to customize the application, you can modify the `region_list` variable to include the regions you want to analyze. You can also modify the `sns.set` line to change the style of the plots generated by the application.

## Functions

The `cost_of_capital_func` module contains the following functions used by the `app.py` program:

-   `get_factor_links(region)`: Returns the file paths for the factor and momentum data for a given region.
-   `download_zip(url, file_path)`: Downloads a ZIP file from the given URL and extracts the contents to the given file path.
-   `clean_data(df)`: Cleans the given DataFrame by dropping null values and converting the Date column to a datetime object.
-   `merge_factors_and_momentum(factors_df, momentum_df)`: Merges the factor and momentum DataFrames on the Date column.
-   `download_historical_prices(ticker, start_date, end_date)`: Downloads historical prices for the given stock ticker symbol using the yfinance library and returns a DataFrame containing the Date and Adjusted Close columns.
-   `run_regression(df)`: Runs a regression on the given DataFrame using the Mkt-RF, SMB, and HML columns as independent variables and the RI-RF column as the dependent variable. Returns the calculated model, Alpha, Beta_Mkt, Beta_SMB, Beta_HML, and Rf values.

These functions are not included in the `app.py` file but are imported using the `from cost_of_capital_func import *` line at the beginning of the file.

Note that it is recommended to place these functions in a separate Python file and import them into the `app.py` file to keep the code organized and easier to maintain.

## Octave API

This API performs a stock factor analysis using data from Kenneth French's website. The analysis calculates the alpha and beta values of a stock, based on its historical prices and factor returns. The API provides a list of available regions to choose from, and accepts a stock ticker symbol as input.

### Endpoints

#### `GET /regions`

Returns a list of available regions to perform stock factor analysis on.

**Example Request:**

`GET http://localhost:8000/regions`

**Example Response:**

```json
[    
    "North America", "Europe", 
    "Japan", "Asia Pacific ex Japan", 
    "Developed ex US", "Developed Markets"
]
```
#### `GET /analyze?region=<region>&ticker=<ticker>`

Performs a stock factor analysis for the given region and ticker symbol. Returns a JSON object containing the alpha and beta values, as well as plot data for the factor returns, momentum returns, historical stock returns, and fitted vs. actual returns.

**Query Parameters:**

-   `region` (string, required): The region to perform analysis on. Must be one of the available regions returned by `/regions`.
-   `ticker` (string, required): The stock ticker symbol to perform analysis on.

**Example Request:**

`
GET http://localhost:8000/analyze?region=North%20America&ticker=AAPL
`

**Example Response:**

```json
{
    "Alpha": 0.004208,     
    "Beta_Mkt": 1.191542,    
    "Beta_SMB": -0.149313,     
    "Beta_HML": -0.157637,    
    "Rf": 0.000215
}
```