# Using xsv join

The PPP data as provided has a `NAICSCode` column, which is what we use for (kind of) categorizing the loan applicants:

<img src="assets/images/ppp-naics-code-column.png" alt="ppp-naics-code-column.png">

However, most of us don't have memorized what those 6-digit codes correspond to. So for every code we want to know about, we manually look up on [naics.com/search](naics.com/search) or look it up on the [official spreadsheet of codes](https://docs.google.com/spreadsheets/d/1AkKFHJb1GSOmPTzNyPsg3swulqbmwm8hGZVW0J-5tk8/edit#gid=1223701275) from the Census Bureau:

<img src="assets/images/naics-lookup-code.png" alt="naics-lookup-code.png">

Obviously doing these lookups is very annoying if you're doing more than a handful. For the purposes of data exploration, Wouldn't it be nice if the human-readable "title" for each code was a column in the PPP loan data, like this?

<img src="assets/images/ppp-naics-code-title-joined.png" alt="ppp-naics-code-title-joined.png">

Because real-world data is often only usable when joining it with other reference data/tables, I'm usually doing SQL pretty early in the data exploration and wrangling process. For example, to create the joined data in the above screenshot, I'd import the CSV files of PPP loans and NAICS codes into a SQLite database, and then execute something like this:

```sql
SELECT *
FROM ppp_loans
LEFT JOIN naics_codes
    ON ppp_loans.NAICSCode = naics_codes.naics_code;
```

But luckily for us, the `xsv join` function can perform this kind of simple single-column join between 2 files:


```
Usage:
    xsv join [options] <columns1> <input1> <columns2> <input2>
```





```sh
$ xsv join --left 'NAICSCode' ppp-loans.csv \
        'naics_code' naics-codes.csv \
    | xsv select 'LoanRange,BusinessName,naics_code,naics_title,Address-CD' \
    > ppp-loans-joined.csv
```
