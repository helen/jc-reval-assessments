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

1. Grab the latest PDF from http://www.asinj.com/revaluation.asp?p=current&id=359 (under `Assessment Lists`).
2. Convert from PDF to Excel using something like https://www.pdftoexcel.com/ (if you can find one that does a better job of deleting repeated headers, then you can skip the next step).
3. Open in Excel/Numbers/whatever and filter out rows that are repeated headers. In Numbers, this is most easily accomplished by filtering them out and copying the results into another sheet. I believe Excel will actually allow you to delete the filtered results in a sane way - Numbers actually deletes rows in between if you shift-select. It's madness.
4. Delete the first 3 columns and the last (empty) column.
5. Export to CSV.
6. Remove the headers from the CSV and save it in this repo as `data-raw.csv`.
7. Convert to `JSON - Row Arrays` using https://shancarter.github.io/mr-data-converter/
8. Check for validity using https://jsonlint.com/ (it will probably fail at this point).
9. Do a find-and-replace on the CSV to replace `,n/a,` with `,"n/a",`.
10. Re-lint the JSON.
11. Assuming it passes this time, save as `data.txt` in this repo.
12. Update the date in `index.html`.
13. Commit and push!
