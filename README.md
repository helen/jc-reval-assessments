# Jersey City 2018 Reval Assessment Data

Jersey City is undergoing its first complete property reval since 1988. Appraisal Systems has been putting the assessment information online but as PDFs, which are not very accessible or parseable. This repo is an effort to present the data in a more web-friendly and universally searchable format, as well as possibly using the data for further analysis and visualizations.

Some ideas I have include:

* Loading data into a web table view more dynamically (the current setup probably won't scale much further).
* Heatmaps of percentage change in assessed value and estimated taxes.
* Breakdowns by neighborhood and ward.

## What does this even mean?

Well, in short, people's assessed property values are changing, both in absolute terms and relative to other properties in Jersey City, which changes their tax burden. Brigid D'Souza over at Civic Parent has [a great set of resources](https://civicparent.org/jerseycity-revaluation-lets-getcivic/).

## Why do you care?

I own a home in Ward A. :)

## Current updating process

This is all very hacky but also fairly fast, so for reference:

1. Grab the latest spreadsheet from http://www.asinj.com/revaluation.asp?p=current&id=359 (under `Assessment Lists`).
2. Delete the first 3 columns and any empty rows and columns.
3. Make sure the SalesDate column is formatted as `M/D/YYYY` (no leading zeroes on the month or day).
3. Export to CSV.
4. Remove the headers from the CSV and save it in this repo as `data-raw.csv`.
5. Do a find-and-replace on the CSV to replace any extra whitespace (mostly on PropLocation): `\s+,` replaced with `,`. There is one outlier at this time that's just easier to manually fix - `"299 FOURTH ST., 61 COLES"`.
6. Convert to `JSON - Row Arrays` using https://shancarter.github.io/mr-data-converter/
7. Check for validity using https://jsonlint.com/ (it will probably fail at this point).
8. Do a find-and-replace on the JSON to replace `,n/a,` with `,"n/a",`.
9. Do another find-and-replace to replace square footage values like `1.234` with `1,234`. In Sublime Text using regex matches, this is replacing `,([0-9]+)\.([0-9]+),` with `,"$1,$2",`.
10. Re-lint the JSON.
11. Assuming it passes this time, save the JSON as `data.txt` in this repo.
12. Update the date in `index.html`.
13. Commit and push!
