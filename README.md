
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

bash ##pip install pandas requests


## Usage
Open the Jupyter Notebook:

```HTML

jupyter notebook XML Data Parser.ipynb

```


## Data Preparation
Step 1: Identify the Required Data
Determine what data you need by downloading corresponding auction dates from TreasuryDirect's auction query. Download and save them as three CSV files:

Securities_Bill.csv
Securities_Note.csv
Securities_Bond.csv
These files should contain auction dates sorted by security type for a clean time span.

Step 2: Sort Out All the Terms
Example terms for Securities_Note.csv:

python
```HTML
['1-Year 10-Month', '1-Year 11-Month', '10-Year', '2-Year', '3-Year', '4-Year 10-Month', '4-Year 4-Month', '4-Year 6-Month', '4-Year 8-Month', '5-Year', '6-Year 10-Month', '6-Year 4-Month', '6-Year 7-Month', '7-Year', '9-Year 10-Month', '9-Year 11-Month', '9-Year 4-Month', '9-Year 8-Month', '9-Year 9-Month']
```



## Function Definitions
- Download XML
python
```HTML
def parse_xml(file_path):
    # Function to parse XML file
    return ElementTree object
-->
```

- Parse XML
python
```HTML
def parse_xml(file_path):
    # Function to parse XML file
    return ElementTree object
```

- Extract Information
python
```HTML
def extract_info(tree):
    root = tree.getroot()
    data = {}
    auction_announcement = root.find("AuctionAnnouncement")
    if auction_announcement is not None:
        data = {
            "SecurityTermWeekYear": auction_announcement.findtext("SecurityTermWeekYear"),
            "SecurityTermDayMonth": auction_announcement.findtext("SecurityTermDayMonth"),
            "SecurityType": auction_announcement.findtext("SecurityType"),
            "CUSIP": auction_announcement.findtext("CUSIP"),
            "BidToCoverRatio": auction_announcement.findtext("BidToCoverRatio")
        }
    return data


def extract_info(tree):
    root = tree.getroot()
    data = {}
    auction_announcement = root.find("AuctionAnnouncement")
    if auction_announcement is not None:
        data = {
            "SecurityTermWeekYear": auction_announcement.findtext("SecurityTermWeekYear"),
            "SecurityTermDayMonth": auction_announcement.findtext("SecurityTermDayMonth"),
            "SecurityType": auction_announcement.findtext("SecurityType"),
            "CUSIP": auction_announcement.findtext("CUSIP"),
            "BidToCoverRatio": auction_announcement.findtext("BidToCoverRatio")
        }
    return data
```

## User Interaction
Prompt the user for the security type and term (year or week):

python
```HTML
security_type = input("Enter the security type (e.g. Bond, Bill, Note): ")
term_type = input("Enter the term type (year/week): ").strip().lower()
term_value = int(input("Enter the term value (e.g. 1, 2, 3, 5, 7, 10, 20, 30 for year or 4, 8, 13, 17, 26, 52 for week): "))
```


# Determine the CSV file path based on the security type
csv_file_path = f'Securities_{security_type}.csv'

# Check if the CSV file exists
if not os.path.exists(csv_file_path):
    print(f"CSV file for {security_type} does not exist: {csv_file_path}")
    exit(1)

# Read the CSV file and extract the Auction Dates
df = pd.read_csv(csv_file_path)
auction_dates = df['Auction Date'].dropna().unique()

## Download and process each XML file for the extracted Auction Dates
Determine the CSV file path based on the security type. Check if the CSV file exists and read the CSV file to extract the Auction Dates. Download and process each XML file for the extracted Auction Dates.

```HTML
# Determine the CSV file path based on the security type
csv_file_path = f'Securities_{security_type}.csv'

# Check if the CSV file exists
if not os.path.exists(csv_file_path):
    print(f"CSV file for {security_type} does not exist: {csv_file_path}")
    exit(1)

# Read the CSV file and extract the Auction Dates
df = pd.read_csv(csv_file_path)
auction_dates = df['Auction Date'].dropna().unique()

# Download and process each XML file for the extracted Auction Dates
for auction_date in auction_dates:
    date_str = convert_date(auction_date)  # Convert date to YYYYMMDD format
    for suffix in range(1, 4):  # Try suffixes 1, 2, 3
        xml_url = base_url.format(date_str, suffix)
        file_name = os.path.join(output_dir, f'R_{date_str}_{suffix}.xml')
        if download_xml(xml_url, file_name):
            if is_security_term(file_name, term_type, term_value):
                new_file_name = os.path.join(output_dir, f'{term_value}_{term_type.upper()}_R_{date_str}_{suffix}.xml')
                os.rename(file_name, new_file_name)
                print(f"Renamed to: {new_file_name}")

                tree = parse_xml(new_file_name)
                data = extract_info(tree)
                data["Date"] = date_str  # Adding the date for reference
                df_fin = pd.concat([df_fin, pd.DataFrame([data])], ignore_index=True)

output_csv = f"{security_type}_results.csv"
df_fin.to_csv(output_csv, index=False)

```

## Security Term Check
python
```HTML
def is_security_term(file_path, term_type, term_value):
    pattern = re.compile(f"<SecurityTermWeekYear>{term_value}-{term_type.upper()}</SecurityTermWeekYear>")
    with open(file_path, 'r') as file:
        for line in file:
            if pattern.search(line):
                return True
            if term_type.lower() == "year" and term_value in {5, 7, 10}:
                for sub_year in range(term_value-1, term_value-2, -1):
                    if re.search(f"<SecurityTermWeekYear>{sub_year}-YEAR</SecurityTermWeekYear>", line):
                        return True
    return False
```


## Advantages & Disadvantages
- Advantages:

Clear user interaction and easy to check the data.
User-friendly interface for downloading specific notes.

- Disadvantages:

Manual input required each time.
Can be automated further with bash scripting for efficiency.

## Notes
Ensure the CSV file with auction dates is available in the working directory.
Update the base_url variable to point to the correct XML file source.
To add different time units like weeks, add corresponding code lines in the is_security_term function.
