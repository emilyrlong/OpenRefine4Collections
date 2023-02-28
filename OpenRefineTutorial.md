# OpenRefine for Collections Data Tutorial

## Getting Started

First, make sure you've read the [README](https://github.com/emilyrlong/OpenRefine4Collections/blob/main/README.md) in this GitHub to download OpenRefine and the collections data for this tutorial.

Open the OpenRefine app in a browser with the link: [http://127.0.0.1:3333](http://127.0.0.1:3333). In OpenRefine,...

### 1. Create a new project and upload the data
* Create Project > This Computer > Choose files
* Upload the [VA_CeramicObjects.csv](https://github.com/emilyrlong/OpenRefine4Collections/blob/main/VA_CeramicObjects.csv) file and hit ‘Next’

### 2. Change the project and data load settings
* Change the Project Name if you like
* Does the data look wonky? In the “CSV / TSV / separator-based files” tab, click
“commas (CSV)”
* Keep the “Trim leading & trailing whitespace...” button checked
* Make sure the Character Encoding is UTF-8
* Make sure that the “Parse Next _1_ line(s) as column headers” is checked
  * Note: If your CSV doesn’t have column names, un-check the “Parse Next _ line(s) as column headers” button, then you can add the column names in a list.
* Then click Create Project

### 3. Get familiar with viewing your data
You can view the rows in groups of 5, 10, 25 or 50, and use the previous/next buttons to move between row pages.

Most of OpenRefine’s tools can be found by clicking the drop-down arrows at the top of each column. Take a moment to check out some of the operations in the drop-down menu. The following are handy to know:
* Rename columns by clicking the drop-down menu arrow > Edit column > Rename this column
* Remove columns by clicking the arrow > Edit column > Remove this column

## Text Facet

### accessionNumber
This is a reference number for when the object was accessioned by the museum, and we would expect this reference to be unique.
1. Use a Text Facet to look at the accession numbers. Sort the facet by count. 
accessionNumber drop-down button > Facet > Text Facet > Sort by count
There are 429 numbers but 498 rows in the data, indicating that there are duplicates.
2. Remove the duplicate rows
a. Sort the accessionNumber column (Drop-down > Sort... > Sort by text)
b. This will temporarily sort the data, and a Sort button will pop up above the page. Click Sort then Reorder rows permanently
c. ...

### accessionYear
1. Convert the accessionYear to a number and use a text facet to see the distribution of years.
Edit Cells > Common Transforms > to number
2. Create a new column called accessionDate which converts the year into a full date. Use a timeline facet to look at the data.
Edit Column > Add column based on this column... > Name the column 'accessionDate' and add the GREL expression ``value.toDate('Y-M-d')``

### Use Clustering for the sampleMaterial column
OpenRefine can automatically detect values that just differ my capitalisation or punctuation, and you can easily update the values to match (e.g, Ceramic and ceramic).
Cluster > Check the 'Merge?' button for all relevant clusters > Merge selected & Close
