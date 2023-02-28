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

## Demo

Most of OpenRefine’s tools can be found by clicking the drop-down arrows at the top of each column. Take a moment to check out some of the operations in the drop-down menu. The following are handy to know:
* Rename columns by clicking the drop-down menu arrow > Edit column > Rename this column
* Remove columns by clicking the arrow > Edit column > Remove this column 
* Facets group the data giving the unique values and the counts of how many times they occurred
* Text filters let you search and filter for values in columns

Example:
* Use a Text Facet or Text Filter to show that the objectContentWarning and imageContentWarning columns only contain false values
* Remove these columns

## For You to Complete...

### accessionNumber
This is a reference number for when the object was accessioned by the museum, and we would expect this reference to be unique. Use a Text Facet to look at the accession numbers. Sort the facet by count. 

<details open>
<summary>Want a hint?</summary>
accessionNumber drop-down button > Facet > Text Facet > Sort by count
</details>

<details open>
<summary>About duplicate data...</summary>
There are 433 accession numbers and 433 rows in the data. But originally, there were duplicates (see <a href="https://github.com/emilyrlong/OpenRefine4Collections/blob/main/VA_CeramicObjects_Duplicates.csv">VA_CeramicObjects_Duplicates.csv</a>). It was easier to remove these duplicates in Python, but you can use this other dataset and try another OpenRefine [tutorial](https://kb.refinepro.com/2011/08/remove-duplicate.html) to remove the duplicates yourself.
</details>

### accessionYear
1. Convert the accessionYear to a number and use a text facet to see the distribution of years.
 * Edit Cells > Common Transforms > to number
 * Facet > Text Facet 
2. Create a new column called accessionDate which converts the year into a full date. Use a timeline facet to look at the data.
 * Edit Column > Add column based on this column... > Name the column 'accessionDate' and add the GREL expression ``value.toDate('Y-M-d')``

### systemNumber
This is the reference number for the object in the V&A [Explore the Collections](https://www.vam.ac.uk/collections?type=featured) website. By appending the systemNumber to the URL ``https://collections.vam.ac.uk/item/``, you can create a URL that links to the object's images and details.

1. Create a URL column
* Edit Column > Add column based on this column... > Name the column 'objectURL' and add the GREL expression `"https://collections.vam.ac.uk/item/" + value`

### objectType

### primaryTitle

### primaryPlace

### primaryMaker Name

### primaryMaker Association

### primaryDate



### primaryImageId
This ID number can be used to find the JPG images of the objects where available. See the V&A developer's guide for images [here](https://developers.vam.ac.uk/guide/v2/images/introduction.html).
1. Create an image URL column
* Edit Column > Add column based on this column... > Name the column 'imageURL' and add the GREL expression 
` "https://framemark.vam.ac.uk/collections/" + value + "/full/!600,400/0/default.jpg" ` 

### sampleMaterial
Use Clustering for the sampleMaterial column
OpenRefine can automatically detect values that just differ by capitalisation or punctuation, and you can easily update the values to match (e.g, Ceramic and ceramic).
Cluster > Check the 'Merge?' button for all relevant clusters > Merge selected & Close

### sampleTechnique


### sampleStyle

### currentLocation displayName
