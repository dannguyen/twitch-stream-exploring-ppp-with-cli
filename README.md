# How to navigate and wrangle the PPP Loan Data from the command-line (xsv and ripgrep)

going to try to twitch stream this. Here are some links to notes and whatever etc.


## Tools

- **ripgrep** is like grep, but faster and more flexible: https://github.com/BurntSushi/ripgrep
- **xsv** is like csvkit, but faster: https://github.com/BurntSushi/xsv

Both tools are authored by [Andrew Gallant aka BurntSushi](https://github.com/BurntSushi) and both are written in Rust, which makes them generally easier to install across any platform.


Microsoft Excel:

```
alias xlopen="open -a 'Microsoft Excel'"
```



## The data

### SBA Paycheck Protection Program Loan Level Data

Since the actual unzipped CSV file is too big to save as-is in this repo, I've only saved it as a ZIP file, which you'll have to unzip:

Local copy: [ppp-150k-plus.zip](data/ppp-150k-plus.zip)
Original source (Box Drive): https://sba.app.box.com/s/tvb0v5i57oa8gc6b5dcm9cyw7y2ms6pp
Landing page: https://home.treasury.gov/policy-issues/cares-act/assistance-for-small-businesses/sba-paycheck-protection-program-loan-level-data

For the purposes of this exercise, you'll have to unzip `ppp-150k-plus.zip` and rename the file `150k plus/PPP Data 150k plus.csv` to just: `ppp-loans.csv`


For convenience's sake, I've also created a few sample selections of the data that are browsable on Google Sheets:

- New York PPP records: [sample.ppp-loans-ny.csv](data/sample.ppp-loans-ny.csv) ([on Google Sheets](https://docs.google.com/spreadsheets/d/1FaAuS4KfSCFKUW14M1EGf3WDWyb1qsPfFhybDHPzNrs/edit?usp=sharing))


You can also check out the data with useful augments, as published by Simon Willison using Datasette: https://sba-loans-covid-19.datasettes.com/loans_150k_plus/foia_150k_plus

Here's more info on what Datasette is and how he wrangled the PPP data: https://github.com/simonw/sba-loans-covid-19-datasette

### North American Industry Classification System:

Note: You can manually look NAICS codes using [naics.com](https://www.naics.com/search/), just to get a feel for what they are. But obviously you want to work from the bulk data to do anything at scale:

Landing page (census.gov): https://www.census.gov/eos/www/naics/downloadables/downloadables.html

There's a couple of files of interest:

##### NAICS 6-digit code file

- original source: https://www.census.gov/eos/www/naics/2017NAICS/2-6%20digit_2017_Codes.xlsx
- local copies:
    - original XLSX: [naics-codes.xlsx](data/naics-codes.xlsx) ([Google Sheets](https://docs.google.com/spreadsheets/d/1AkKFHJb1GSOmPTzNyPsg3swulqbmwm8hGZVW0J-5tk8/edit#gid=1638180595))
    - converted+tidy CSV: [naics-codes.csv](data/naics-codes.csv)([Google Sheets](https://docs.google.com/spreadsheets/d/1AkKFHJb1GSOmPTzNyPsg3swulqbmwm8hGZVW0J-5tk8/edit#gid=1223701275))


##### NAICS descriptions

- local mirror: [naics-descriptions.xlsx](data/naics-descriptions.xlsx) ([on Google Sheets](https://docs.google.com/spreadsheets/d/1495vxQnN0Q49ysZB6jB8vYiZC2TnR-t5Ph4-IAELzBg/edit#gid=1079796598))
- original source: https://www.census.gov/eos/www/naics/2017NAICS/2017_NAICS_Descriptions.xlsx
