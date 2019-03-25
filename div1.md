# NLA OpenRefine Workshop Outline
Compiled and presented by Alissa McCulloch, Australian Monographs

## Background
Hi, for those who don't know me, my name is Alissa McCulloch, I'm in Aus Monos, and I've spent a lot of time over the last six months up to my eyeballs in this thing --> OpenRefine, a data cleaning and editing program that I love, so much, and I want you to love it too!

This workshop is a bit of a test run, I'm running a longer version at [VALA Tech Camp](https://www.vala.org.au/events/vala-tech-camp-2019/camp-sessions-w2-mcculloch/) in a few weeks. The version I'm presenting to you today is focussed more on NLA processes and priorities; in particular, using OpenRefine to enhance MARC records, since that seems like something we're likely to do a lot of (especially with NED coming up etc.)

I have also never run a workshop before in my life, and public speaking is hard for me, so please be nice :) *cross fingers emoji*

(I realised while compiling this session that it may end up being less of a workshop and more of an extended demonstration, to give you a taste of what OpenRefine is capable of. You don't have to do things as I do them, but you can if you like. The datasets are available at [P:\TEMP\amcculloch]. Be aware the MARC dataset is my favourites list I exported from VuFind because it was the only dataset I could get my hands on last Saturday, so you'll get a glimpse of my personal reading habits too, haha.)

## What is OpenRefine and why do I want it?
OpenRefine (formerly Google Refine) is a free and open-source software program for cleaning and editing messy data at scale. Instead of painstakingly editing data fields one at a time, OpenRefine provides some powerful tools to help you clean many thousands of fields at once. These include advanced filtering and faceting abilities, a 'cluster and edit' tool that algorithmically sorts and cleans your data, and a reconciliation function to compare your data with an external source. I personally find it less intimidating than Excel. It also nicely complements MarcEdit (and Terry Reese has done a bit of work in this area to ensure people can use the two tools simultaneously. We'll explore this functionality today).

**OpenRefine, and tools like it, cannot and will not replace cataloguers.** It can help make cataloguing and metadata management faster, easier and more enjoyable. It can also enable tasks like authority control to be undertaken at far greater scale (and, in my view, with far greater accuracy) than is currently being done at NLA. We can use algorithms and automation tools to assist with cataloguing - but there will always be a need for skilled cataloguers to do the work itself. (That's where you come in!)

## Workshop Objectives
By the conclusion of this workshop participants will be familiar with how to:

-   Import and export data in and out of OpenRefine
-   Facet, filter, cluster and edit data
-   Transform data using GREL (General Refine Expression Language)
-   Reconcile data from an external source (VIAF)

### 1. Setup
Ensure OpenRefine has been [downloaded](http://openrefine.org/download) and is working on your machine (Note to self: ask IT to deploy this on the training room computers).

Double-click on the OpenRefine icon (the diamond) and after a few seconds the application should open in your web browser. If it doesn't open, go to http://127.0.0.1:3333

If nothing works (IT haven't whitelisted, etc) use this as a last resort [https://labs.cognitiveclass.ai] (people will have to setup accounts though, which is not ideal)

### 2. Getting data into OpenRefine
OpenRefine accepts data in a variety of formats: TSV, CSV, Excel (.xls and .xlsx), JSON, XML, RDF as XML, and Google Data documents, as well as data directly imported from the web or a Google Sheets spreadsheet.

OpenRefine claims to be able to work with MARC files, but there's a nasty catch!! If you try to feed OpenRefine a MARC file, it will convert it to MARCXML, which is considerably more difficult to work with. I **strongly recommend** you convert a MARC file to TSV first, using MarcEdit's OpenRefine Data Transfer tool. (This is not a MarcEdit workshop, but you don't need to know much else about MarcEdit in order to use this tool.)

[Demo the tool]

For more info on how MarcEdit can work with OpenRefine, see [this](https://blog.reeset.net/archives/1873) explainer from Terry Reese.

### 3. Getting to know your data
During this workshop I'll be switching between two instances of OpenRefine: one a direct import of a MARC file, and one that looks a bit more like a regular spreadsheet. Some features will be more useful with one kind of data over another. MARC data is very rows-based, while OpenRefine is more of a columns-based application. Editing MARC files and regular spreadsheets are a bit different, so I wanted to showcase both--let me know if the switching is getting confusing, and I'll stick to one. 

When importing TSV from MarcEdit the column setup is done for you--you'll see columns named 'RecordNumber', 'Tags', 'Indicators' and 'Content'. Make sure these still look the same by the time you're finished enhancing your data, otherwise they won't import properly back into MarcEdit, and hence to a binary MARC file. 

Similarly, if you're importing an Excel spreadsheet, any column headers will accompany the data into OpenRefine. 

[Open DET CRC xls in OR] 

By way of example: this is a real dataset that was seemingly exported from DLC--we had an organisation called Deep Exploration Technologies Cooperative Research Centre, or DET CRC, individually edeposit 485 items when their funding ran out and they had to shut up shop. Aus Monos decided it wasn't worth our time individually cataloguing almost 500 highly technical works on mining and petrochemical exploration, so this dataset was given to me to work some bulk magic on. (If I were doing this again, I'd ask for the data in MARC format - this is more of an illustration of what can be done with a regular spreadsheet) The author data in particular was riddled with errors, so I focussed my energies on name authority control... on which more later. :)

#### Rows versus records
In OpenRefine, one *record* can have multiple *rows*. Think of it as one MARC record having multiple fields - 700, 650, etc. If you have a dataset with multiple values in the one cell, it can be very useful to separate them out into their own *rows* within the one *record*.
[Demo join/split on DET CRC/Contributor]
1. Observe the Contributor column - there's multiple values in here, split by a semicolon.
2. Edit cells > Split multi-valued cells > Use a semicolon (;) as the separator
3. Suddenly I have 2000+ rows whaaaaat. But still only 484 records! I can switch between views by toggling 'Show as' in the top left hand corner.
4. To join back into a multi-valued cell: Edit cells > Join multi-valued cells > insert separator
5. To split these into columns (not rows): take your multivalued cell > Edit columns > Split into several columns... > use ; as separator

### 4. Simple cleaning without code: facets and filters
(Decide whether to illustrate this with the DET CRC spreadsheet or the MARC one... possibly both?)

Facets and filters help you narrow down what portions of your dataset you'd like to edit. *They are not editing tools* in and of themselves, but they can enable you to do bulk data edits without needing to know how to code. (There is code later on, but it's a bit of a steep learning curve.)

Text filters look like a search box - anything you type here will be searched for, and rows matching your query will be displayed. You can use regular expressions here too.

#### Types of facets
* Text facets
* Numeric and Timeline facets - display graphs instead of lists, for numbers and dates respectively
* Word facet - this breaks down text into words and counts the number of records each word appears in
* Duplicates facet - this results in a binary facet of ‘true’ or ‘false’. Rows appear in the ‘true’ facet if the value in the selected column is an exact match for a value in the same column in another row
* Text length facet - creates a numeric facet based on the length (number of characters) of the text in each row for the selected column. This can be useful for spotting incorrect or unusual data in a field where specific lengths are expected (e.g. if the values are expected to be years, any row with a text length more than 4 for that column is likely to be incorrect)
* Facet by blank - a binary facet of ‘true’ or ‘false’. Rows appear in the ‘true’ facet if they have no data present in that column. This is useful when looking for rows missing key data. **We'll be using this later**

#### Example 1: faceting by tag (MARC data)
Facet > Text facet. You can select multiple values to `include` or `exclude`. 

When editing MARC data, it is necessary to facet by tag in order to achieve anything with the data, as it's all in the one column.

[Demo facet by tag]

**Caution!** Even if you have faceted by tag before working with the Content column, selecting anything in the 'Edit column' menu will indeed edit the entire column, *not just the faceted data*. Ask me how I know this (:

<!--Isolating and editing cells in bulk. Including how to do these things with regex (touch on this briefly). This specifies 'without code' in case some attendees are apprehensive about code. There is code later, but this might help people feel like they can Do The Thing without needing to code :)
-->

#### Example 2: facet by author (DET CRC)
Facet > Text facet > scroll down to DET CRC. I can easily bulk edit this by hovering my cursor to the right and selecting 'edit', making my changes, and selecting 'Apply' / Enter. Bam! All 37 instances of this value in the Creator column have been automagically changed!

Scroll further to admire the six instances of Dyskin's name. Here we have a choice. We can either manually edit these to a preferred form of name such that they all match, or we can try...

### 5. Clustering and editing
AKA the killer feature of OpenRefine. Let the algorithm assist you to normalise your messy data---it will highlight cells that look slightly different but probably mean the same thing and group them together, ready for you to decide which form you want.

I'll demonstrate this with the DET CRC data--despite around 1/3 of the dataset having been catalogued by the time I got my hands on it, there were still inconsistencies in the name headings that OpenRefine was very good at sorting out. (It took a couple of hours, though.)

[Demo cluster + edit with DET CRC creator field]
For demo purposes, check if any of these were editors? Do I need to set the $e data aside? 
1. Text filter > 'editor' > nope. We good.
2. Might bulk-remove all the 'author' instances then... could do a transform (but we're not up to that yet!) `value.replace(', author.','.')`
3. Edit cells > Cluster and edit. Emphasise that this is, at heart, a manual process: we're using algorithmic suggestions to make human decisions on what form of name we want. Because none of the DET CRC name headings had been previously established (i.e. they weren't in an authority file), it's up to the cataloguer to determine the preferred form of name.
4. Once we've exhausted one clustering algorithm, we'll move onto the next. **Be aware of false positives!** It will often try to match things that the cataloguer decides aren't really a match. The clustering tool is a guide only :) 
5. Depending on the size of your dataset, this can take a bit of time. Consider it authority work, only crunchy. :)

### 6. Transforms
In OpenRefine, 'transform' is a noun. Overview of 'common transforms' (there's a menu for these: remove whitespace, upper/lowercase, to null, etc) and less-common transforms (using GREL, or General Refine Expression Language, which can appear scary but is quite a friendly scripting language. Include plenty of examples and some links to canned GREL scripts for common tasks)

GREL looks similar to JavaScript, if you're familiar with that at all, and works in a similar way to Excel functions. I dunno about you but I find Excel functions really intimidating - GREL tends to use words, rather than numbers, and tries to be a bit more user-friendly.

(Refer to the Library Carpentry transformations tutorial [https://librarycarpentry.org/lc-open-refine/07-introduction-to-transformations/index.html])

[Demo case transform on DET CRC/Publisher]
[Demo another one tbc]

### 7. Reconciliation
AKA the other killer feature: matching your data against an external source, in this case reconciling name headings against VIAF. 

MarcEdit does have a name + subject validation utility, but to my knowledge it will *only* reconcile against LCSH and LCNAF. The beauty of the OpenRefine reconciliation tool is that it will reconcile against whatever you tell it to.

However: it will also **only reconcile in plain text!** This means we have to strip the MARC subfields out of the name headings before reconciling. It's inconvenient, and can't be completely automated for reasons you'll see shortly, but it's possible.

See [http://refine.codefork.com/] for instructions on setting up the VIAF target > Reconcile > Start reconciling... > Add standard service > paste URL > leave everything else and click OK

#### In the MARC File
1. Facet by 100 and 700 fields
2. Split cells in Content column into several columns by separator [$e]. Do not delete column, do not guess cell type
3. Text transform on Content column `value.replace('$a','').replace('$d',' ').replace('$q',' ').replace('$c',' ').split('$e')[0]`
4. Reconcile Content column against VIAF People/persons target > 
http://refine.codefork.com/reconcile/viaf
5. Facet by 245 fields too (so you know what you're looking at)
6. Choose appropriate forms of name etc., like in regular name authority control
7. Export as TSV and re-import into OpenRefine (this is because the underlying values *do not change* and GREL can't cope with transforms on both reconciled and unreconciled data, which is what we have. Not very useful for our purposes! This is seemingly a design feature of OpenRefine, and the only workaround I've found is to export and re-import, stripping the underlying values. Sorry for the inconvenience)
8. Text transform on Content column `'$a' + value`
9. Another text transform on Content column `value.replace(' (', '$q(').replace(' 1', '$d1')`
10. Text filter > $q > I can see the only two $q fields that ought to be $c fields are '(Zine creator'), so in this particular example I can either edit those two manually, or run a script. Let's run a script > `value.replace('$q(Zine','$c(Zine')`
11. Manually merge the only one with multiple $e fields because in this case it's easier than the alternative (facet by blank twice). Like anything it's a judgment call as to whether manual editing or scripting is easier
12. Content 2 column > Facet > Customized facets > Facet by blank (True or false - false! it is not blank! there is a value in it)
13. Text transform on Content column to concatenate the two columns `value + "$e" + cells['Content 2'].value`
14. Delete Content 2 column as it's no longer needed
15. Pat yourself on the back

16. (extra) Checking for errant missed full stops: text filter > \.$ [regex] > invert = this shows all fields that do NOT have a full stop at the end. Most of these will be fields with a $d but no $e. Text filter > $d > invert || Text filter > $4 > invert || Transform `value + '.'`

Remembering to do all those steps is finicky and exhausting. Fortunately OpenRefine provides a way to export your edit history so you can re-apply those same steps to another set of data. **Caution!!** This does NOT include setting facets and filters, because those don't actually edit your data, just change the way it's presented to you. If you choose to re-use your edit history this way, make sure your facets are set first. This is extra important when editing MARC data - otherwise you'll end up with a bunch of mis-edited fields!

Undo/Redo tab > Extract > copy and paste into a Notepad or Notepad++ file, save as JSON. To reuse, simply paste it back in

[Demo JSON edit history export]

### 8. Getting data out of OpenRefine
OpenRefine includes a *lot* of options for getting your data back out. You can export in several data formats (CSV, TSV, Excel, ODS, HTML table, direct to a Google Sheet, direct to Wikidata!). 

#### Tips for exporting
I recommend using the 'Custom tabular exporter', paying attention to the following options:
* Untick 'Output nothing for unmatched cells' (if you've been doing reconciliation work)
* Tick 'Ignore facets and filters and export all rows' (it's easy to accidentally leave your facets on, especially when working with MARC data)
* On the Download tab, ensure the character encoding is UTF-8 (special characters can cause problems)
Choose your preferred export format (in this case we want .tsv) and click Download.

'You can also export a whole OpenRefine project as a .tar.gz file and then someone else can import it into their OpenRefine. The exported file will contain not only the data but also the history of changes. This way, the person who receives your project file knows exactly what operations you have performed, and can even undo them.' (as per the [Exporters](https://github.com/OpenRefine/OpenRefine/wiki/Exporters) tutorial)

Exporting projects and exporting data are therefore not the same thing! :D

### Getting MARC data back through MarcEdit and into Voyager where it belongs
Export as TSV as outlined above > open MarcEdit > OpenRefine Data Transfer (ensure the dialogue box is showing tsv files) > Compile file into MARC

[Demo this process in OpenRefine and MarcEdit]

## Further reading
* [Library Carpentry OpenRefine lesson](https://librarycarpentry.org/lc-open-refine/), on which this workshop is based (and some of the verbiage is pinched under their CC-BY 4.0 license, cheers)
* [A worked example of fixing problem MARC data](http://www.meanboyfriend.com/overdue_ideas/tag/fixmarc/?orderby=date&order=ASC), a 5-part series by OpenRefine wizard Owen Stephens
* [OpenRefine wiki](https://github.com/OpenRefine/OpenRefine/wiki), an in-depth technical manual
* [OpenRefine recipes](https://github.com/OpenRefine/OpenRefine/wiki/Recipes), a list of useful assorted GREL scripts
