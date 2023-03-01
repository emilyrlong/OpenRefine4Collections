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

<details>
<summary>Want a hint?</summary>
accessionNumber drop-down button > Facet > Text Facet > Sort by count
</details>

<details>
<summary>About duplicate data...</summary>
There are 433 accession numbers and 433 rows in the data. But originally, there were duplicates (see <a href="https://github.com/emilyrlong/OpenRefine4Collections/blob/main/VA_CeramicObjects_Duplicates.csv">VA_CeramicObjects_Duplicates.csv</a>). It was easier to remove these duplicates in Python, but you can use this other dataset and try another OpenRefine <a href="https://kb.refinepro.com/2011/08/remove-duplicate.html">tutorial</a> to remove the duplicates yourself.
</details>

### accessionYear
1. Convert the accessionYear to a number and use a text facet to see the distribution of years.

<details>
<summary>Want a hint?</summary>
<ul>
  <li>Edit Cells > Common Transforms > to number</li>
  <li>Facet > Text Facet</li> 
</ul>
</details>

2. Create a new column called accessionDate which converts the year into a full date. Use a timeline facet to look at the data.

<details>
<summary>Want a hint?</summary>
<ul>
  <li> Edit Column > Add column based on this column... > Name the column 'accessionDate' and add the GREL expression <b>value.toDate('Y-M-d')</b></li>
  <li>Facet > Timeline Facet</li> 
</ul>
</details>

### systemNumber
This is the reference number for the object in the V&A [Explore the Collections](https://www.vam.ac.uk/collections?type=featured) website. By appending the systemNumber to the URL ``https://collections.vam.ac.uk/item/``, you can create a URL that links to the object's images and details.

**Create a URL column**

<details>
<summary>Want a hint?</summary>
Edit Column > Add column based on this column... > Name the column 'objectURL' and add the GREL expression <b>"https://collections.vam.ac.uk/item/" + value</b>
</details>

### objectType
Use a text facet to explore the object types. Use the cluster feature to find object types that just differ by capitalisation. The clustering can automatically detect values that just differ by capitalisation or punctuation, and you can easily update the values to match (e.g, Ceramic and ceramic).

<details>
<summary>Want a hint?</summary>
objectType > Facet > Text facet > Cluster > Check Merge? for all clusters that match > Merge selected & Close
</details>

You may want to create a new column called objectCategory to refine these further.

<details>
<summary>Want a hint?</summary>
Edit Column > Add column based on this column... > Name the column 'objectCategory' and leave the GREL expression as 'value'
</details>

### primaryTitle
This column has lots of blank values, but use a text facet to explore the data a bit.

### primaryPlace
This column names the place of origin of the objects, but these can be cities or countries. You could create a column for Country or City using the text facet. See the objectType hints for more info.

### primaryMaker Name
You could use a text facet and clustering to clean up these names. Look for some fun facts!

### primaryMaker Association
You could use a text facet and clustering to clean up these associations.

### primaryDate
The dates include ranges, centuries, and single years. You could create a Year and Century column from this data.

<details>
<summary>1. Create a Year column</summary>
Create a new column and use if statements in a GREL function:
 <br>
if(value.contains('entury'),null,
if(value.contains('-'),value.splitByCharType()[0],
if(value.contains(' '),value.splitByCharType()[3],
if(value.contains('.'),value.splitByCharType()[2],
if(value.contains('s'),value.splitByCharType()[0],value))))).toNumber()
 <br>
Note that this doesn't cover all the different cases, but then you can use a text facet to clean it up.
</details>

<details>
<summary>2. Create a CenturyYear column based on the Year column </summary>
substring(value.toString(),0,2).toNumber() + 1
<br>
Then convert to text with Edit cells > Common transforms > To text
</details>

<details>
<summary>3. Create a Century column based on the primaryDate column </summary>
if(value.contains('entury'),
value.toLowercase().replace('mid ','').replace(' century','').replace('th','').replace('late','').replace('to','').trim(),
null)
 <br> <br>
 Use a text facet to clean up the outlier cases.
</details>

<details>
<summary>4. Join the CenturyYear and Century column </summary>
Edit column > Join columns... > Select the century column > Write result in new column named 'AllCentury'
</details>

### primaryImageId
This ID number can be used to find the JPG images of the objects where available. See the V&A developer's guide for images [here](https://developers.vam.ac.uk/guide/v2/images/introduction.html).

**Create an image URL column**

<details>
<summary>Hint:</summary>
Edit Column > Add column based on this column... > Name the column 'imageURL' and add the GREL expression <b>"https://framemark.vam.ac.uk/collections/" + value + "/full/!600,400/0/default.jpg"</b>
</details>

### sampleMaterial
Use a text facet and clustering to clean up this column.

### sampleTechnique
Use a text facet and clustering to clean up this column.

### sampleStyle
There isn't much data in this column, but use a tex facet to explore the data.

### currentLocation displayName
This data could be used in a few ways. Use a text facet to explore a bit
* Clean up the location names (e.g., clustering for 'in store')
* Create a variable called 'Site' to split South Kensington and Wedgwood

<details>
<summary>Gallery Name (e.g., Jewellery) or In store</summary>
if(value.contains('In store'),'In store',
if(value.contains(','),value.substring(0,value.indexOf(',')),null))
</details>
 
<details>
<summary>Gallery Number (e.g., Room 91)</summary>
if(value.contains('Room'),
if(value.contains('Prints'),null,
value.substring(value.indexOf('Room')+5).trim().splitByCharType()[0]),
null)
</details>
 
## Export Data

When your data is cleaned up, click the Export drop-down button in the top right corner, and choose a file format to export in. For visualising, a CSV, xlsx, or Google Sheets could be handy.
