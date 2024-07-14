
# XML Data Parser

This repository contains a Jupyter Notebook for parsing and extracting data from XML files available at [TreasuryDirect.gov](https://www.treasurydirect.gov/auctions/announcements-data-results/announcement-results-press-releases/).
The code downloads XML files, processes them based on specified security types and terms, and extracts relevant information to save into a CSV file.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Data Preparation](#data-preparation)
- [Code Explanation](#code-explanation)
- [Function Definitions](#function-definitions)
- [User Interaction](#user-interaction)
- [Downloading and Processing XML Files](#downloading-and-processing-xml-files)
- [Security Term Check](#security-term-check)
- [Advantages & Disadvantages](#advantages--disadvantages)
- [Notes](#notes)

## Installation

To run the notebook you need to have the following dependencies installed:
- Python 3.x
- pandas
- requests
- xml.etree.ElementTree

You can install these dependencies using pip:

```bash
pip install pandas requests


## Usage

Open the Jupyter Notebook:



Run the notebook cells to execute the code.

## Data Preparation

### Step 1: Identify the Required Data
Determine what data you need by downloading corresponding auction dates from [TreasuryDirect's auction query](https://www.treasurydirect.gov/auctions/auction-query/). Download and save them as three CSV files:
- `Securities_Bill.csv`
- `Securities_Note.csv`
- `Securities_Bond.csv`


These files should contain auction dates sorted by security type for a clean time span.

### Step 2: Sort Out All the Terms
Example terms for `Securities_Note.csv`:
['1-Year 10-Month', '1-Year 11-Month', '10-Year', '2-Year', '3-Year', '4-Year 10-Month', '4-Year 4-Month', '4-Year 6-Month', '4-Year 8-Month', '5-Year', '6-Year 10-Month', '6-Year 4-Month', '6-Year 7-Month', '7-Year', '9-Year 10-Month', '9-Year 11-Month', '9-Year 4-Month', '9-Year 8-Month', '9-Year 9-Month']


Use these auction dates to download XML files for specific dates.

## Code Explanation

### Function Definitions

#### Download XML

Function to download XML file from URL.

#### Parse XML

Function to parse XML file and return ElementTree object.

#### Extract Information

Function to extract information from parsed XML tree.

## User Interaction

Prompt the user for the security type and term (year or week):

Enter the security type (e.g. Bond, Bill, Note):
Enter the term type (year/week):
Enter the term value (e.g. 1, 2, 3, 5, 7, 10, 20, 30 for year or 4, 8, 13, 17, 26, 52 for week):


## Downloading and Processing XML Files

Determine the CSV file path based on the security type. Check if the CSV file exists and read the CSV file to extract the Auction Dates. Download and process each XML file for the extracted Auction Dates.

## Security Term Check

Function to check if the term in the XML file matches the specified term type and value.

## Advantages & Disadvantages

**Advantages:**
- Clear user interaction and easy to check the data.
- User-friendly interface for downloading specific notes.

**Disadvantages:**
- Manual input required each time.
- Can be automated further with bash scripting for efficiency.

## Notes

- Ensure the CSV file with auction dates is available in the working directory.
- Update the `base_url` variable to point to the correct XML file source.
- To add different time units like weeks, add corresponding code lines in the `is_security_term` function.


