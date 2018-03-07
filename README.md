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
2. Make sure the SalesDate column is formatted as `M/D/YYYY` (no leading zeroes on the month or day).
3. Export to CSV.
4. Do a find-and-replace on the CSV to replace any extra whitespace (mostly on PropLocation): `\s+,` replaced with `,` and `\s+",` replaced with `",`. There is also one error that's just easier to manually fix - `"191 CLENDENNY AVE,"` should become `"191 CLENDENNY AVE."`.
5. Remove the headers from the CSV and save it in this repo as `data-raw.csv`.
6. Convert to `JSON - Row Arrays` using https://shancarter.github.io/mr-data-converter/
7. Do a find-and-replace on the JSON to replace `,n/a,` with `,"n/a",`.
8. Do another find-and-replace to replace square footage values like `1.234` with `1,234`. In Sublime Text using regex matches, this is replacing `,([0-9]+)\.([0-9]+),` with `,"$1,$2",`.
9. Do a more manual find-and-replace of some funky lot numbers, changing from ones that look like `00012  34` to `"00012.34"` (note the additional double quotes). You can find them using `,[0-9]{5}` as the regex. If this becomes extremely common and/or annoying, I'll makes this a proper regex replacement.
10. Check for validity using https://jsonlint.com/ (hopefully it will pass).
11. Assuming it passes, save the JSON as `data.txt` in this repo.
12. Update the date in `index.html`.
13. Commit and push!
