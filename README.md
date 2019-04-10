# Cleaning Data with OpenRefine 💎 Project Guide
Welcome to the VALA Tech Camp 2019 OpenRefine project guide! Congratulations on getting through the teaching part of the workshop. Now it's time to get stuck into some dodgy data 🙂

You can download a copy of the workshop slides [here](https://github.com/lissertations/openrefine/blob/master/Final_slides.pdf). (right-click to save)

Please feel free to ask Alissa or Alexis for help. If you have a question we can't answer (and we're not experts, so it's possible) or you get stuck later on, you're welcome to tweet or email Alissa and she'll get back to you 🐦

twitter: [@lissertations](https://twitter.com/lissertations) | 
email: [hello@lissertations.net](mailto:hello@lissertations.net)

## Download your data ⬇️
You have two choices of project data: a **MARC file (converted into TSV)** and a **regular spreadsheet / CSV file**. Feel free to play around with either or both! Right-click on these links to save them to your computer.

[MARC/TSV file](https://github.com/lissertations/openrefine/blob/master/marc_data.tsv) 

[DOAJ/CSV file](https://github.com/lissertations/openrefine/blob/master/doaj_data.csv)

To get these files into OpenRefine:
1. Right-click on these links to save the files to your desktop. 
2. In [OpenRefine](http://127.0.0.1:3333/), make sure you've selected 'Create Project' and 'Get data from this computer'. Click 'Browse' to locate the file, then click 'Open', then 'Next'.
3. You shouldn't need to change anything on the next screen—ensure OpenRefine is parsing your data as 'CSV / TSV / separator-based files'. Give your project a name if you like, then click 'Create Project'.

If you don't want to try your hand at specific exercises, feel free to have a play around with the different features of OpenRefine. Remember, today we learned how to:
* Import and export data in and out of OpenRefine
* Facet, filter, cluster and edit data
* Transform data using GREL (General Refine Expression Language)
* Reconcile data against an external source (VIAF)

Some of these exercises are the same ones that were demonstrated today. If we can do it, you can do it too 🌠

## MARC data as TSV (marc_data.tsv)
*Focused demonstration of how OpenRefine can be used to enhance MARC data*

This dataset comprises around fifty random MARC files that Alissa pulled from the NLA's catalogue, possibly from her giant to-read pile. 😄

Ordinarily you would need to use a program like [MarcEdit](https://marcedit.reeset.net/downloads) to convert a binary MARC file into a tab-separated values (TSV) file. OpenRefine doesn't deal well with MARC files, so we need to convert it into a format that looks more like a spreadsheet. Terry Reese, the creator fo MarcEdit, has written a [workflow](https://blog.reeset.net/archives/1873) on how you can do this.

Many libraries use both MarcEdit and OpenRefine to clean and improve their data. The two programs complement each other, but some things (such as clustering) are better or easier in one program than the other—it's a bit of trial and error to determine what might work best for you.

### Things to try

#### Facet by tag
Because of the layout of a MARC file within OpenRefine, it's often easiest to look at all the instances of a given field at once, eg. all the 650 fields. Open up a text facet (Facet > Text facet) and select the field you want.

**Caution!** Even if you have faceted by tag before working with the Content column, selecting anything in the 'Edit column' menu will indeed edit the entire column, *not just the faceted data*. We may have learned this the hard way 🙃

Often having two facets can be a great way to spot inconsistencies in your data. For example, put a text facet on the Tags column, then another on the Indicators column. This will show you the indicator combinations for each instance of that particular field. If any shouldn't be there, simply click on the indicator values on the left to pop open a text box, where you can edit them.

#### Bulk edit headings
Let's suppose some of these headings are a little out of date, and we want to replace them. 
1. Put a text facet on the Tags column, and select the 650 field.
2. Put another text facet on the Content column. Don't select anything, but have a look at what's in the Content facet. You can sort by 'name' or by 'count'.
3. To edit a heading, hover over it within the Content facet. An 'edit' button will appear. Click on it. Make whatever edits you desire, then click Apply. Bam! Your headings have been magically changed! 🔮

#### Insert a subfield
You'll see a lot of these records are AACR2-era records, and are missing some of the more modern fields. Here we'll try inserting a $q into 020 fields that don't have one, where the ISBN and the qualifying information are both in the $a field.
1. Put a text facet on the Tags column, and select the 020 field.
2. In the Tags column, go to Edit cells > Transform and run the following script: `value.replace(' (','$q(')`
3. Admire your new subfields! Yay! 😀

#### Fix dodgy UTF-8 encoding
I'm not sure if there is any in this dataset, but you may find that moving data between various programs and systems can result in special characters going a bit off, and 'é'' becomes 'Ã©', among other things.

To rectify, simply run this transformation on the offending column:

`value.reinterpret("utf-8")` 

#### Get subfields out of your name headings before clustering
As mentioned, the excellent Cluster and edit and Reconciliation features only work with plain text (that is, text without MARC subfields). This workflow is quite convoluted, and you don't have to finish it today! This is part of a real workflow that Alissa used at work.

To strip the MARC subfields from the 100 field:
1. Put a text facet on the Tag column, and select 100 from the list on the left
2. If there are $e subfields in here, you'll need to split them out into their own column, to be reattached later. On the Content column, go to Edit cells > Split multi-valued cells, and use '$e' as the separator. Do not delete column, do not guess cell type. This will create a new 'Content 2' column.
3. Run the following transformation on the 'Content' column: 
`value.replace('$a','').replace('$d',' ').replace('$q',' ').replace('$c',' ').split('$e')[0]`
4. Go to Edit cells > Cluster and edit, and cluster away!
5. Adding the subfields back in is slightly convoluted, and there are a couple of things to watch out for:
   * Go to Edit cells > Transform, and paste in the following GREL script `'$a' + value` (this adds the $a to the front of the cell)
   * Repeat, but with this GREL script `value.replace(' (', '$q(').replace(' 1', '$d1')` (this replaces the $d and $q fields. But not everything needs to be a $q!)
10. Let's look at the $q fields. Run a Text filter and put '$q' in the box. You can see the only two $q fields that ought to be $c fields are '(Zine creator'), so here you can either edit those two manually, or run a script. Let's run a script > `value.replace('$q(Zine','$c(Zine')`
11. One of the fields has two $e subfields. Here it's easier to just manually edit the field. It's often a judgment call in OpenRefine as to whether it's more efficient to write a GREL script, or just edit a couple of things manually. 🙂
12. In the Content 2 column, go to Facet > Customized facets > Facet by blank. This will return a Boolean, or 'true or false' value. (Either 'false! it is not blank!' or 'true! there is a value in it')
13. Now run a transform on the Content column to concatenate the two Content columns, or bring them together into one column: `value + "$e" + cells['Content 2'].value`
14. Delete Content 2 column as it's no longer needed: Edit column > Remove this column
15. Pat yourself on the back! That was really finicky but you did it, well done 🎉

## DOAJ data as DSV (doaj_data.csv)
*Broader demonstration of the features and capabilities of OpenRefine for typical spreadsheets*

This dataset (and most of the exercises) are pinched from Library Carpentry. It's a slightly messy bibliographic dataset pulled from the *Directory of Open Access Journals (DOAJ)*. OpenRefine is happiest with data in this kind of spreadsheet format, so you can do lots of cool things with this.

### Things to try

#### Transform to date
1. Make sure you remove all Facets and Filters
2. On the Date column use the dropdown menu to select Edit cells > Common transforms > To date
3. Note how the values are now displayed in green and follow a standard convention for their display format (ISO8601) - this indicates they are now stored as date data types in OpenRefine. We can now carry out functions that are specific to Dates
4. On the Date column dropdown select Edit column > Add column based on this column. Using this function you can create a new column, while preserving the old column
5. In the ‘New column name’ type “Formatted Date”
6. In the ‘Expression’ box type the GREL expression `value.toString("dd MMMM yyyy")`

#### Reconciling publisher names with VIAF IDs
Here we'll use the VIAF Reconciliation service written by [Jeff Chiu](https://twitter.com/absolutelyjeff), via a public service he runs at [http://refine.codefork.com/](http://refine.codefork.com/).

*   In the Publisher column use the dropdown menu to choose ‘Reconcile > Start Reconciling’
    *   We'll need to add the VIAF service in manually, as it doesn't come included. Click ‘Add Standard Service…’ and in the dialogue that appears enter “http://refine.codefork.com/reconcile/viaf”
*   You should now see a heading in the list on the left hand side of the Reconciliation dialogue called “VIAF Reconciliation Service”
*   Click on this to choose to use this reconciliation service
*   In the middle box in the reconciliation dialogue you may get asked what type of ‘entity’ you want to reconcile to - that is, what type of thing are you looking for. The list will vary depending on what reconciliation service you are using.
    *   In this case choose “Corporate Name” (it seems like the VIAF Reconciliation Service is slightly intelligent about this and will only offer options that are relevant)
*   In the box on the righthand side of the reconciliation dialogue you can choose if other columns are used to help the reconciliation service make a match - however it is sometimes hard to tell what use (if any) the reconciliation service makes of these additional columns
*   At the bottom of the reconciliation dialogue there is the option to “Auto-match candidates with high confidence”. This can be a time saver, but in this case you are going to uncheck it, so you can see the results before a match is made
*   Now click ‘Start Reconciling’

Reconciliation is an operation that can take a few minutes if you have many values to look up. However, in this case there are only 6 publishers to check, so it should work quite quickly.

Once the reconciliation has completed two Facets should be created automatically:

*   Publisher: Judgement
*   Publisher: best candidate’s score

These are two of several specific reconciliation facets and actions that you can get from the ‘Reconcile’ menu (from the column drop down menu).

*   Close the ‘Publisher: best candidate’s score’ facet, but leave the ‘Publisher: Judgement’ facet open

If you look at the Publisher column, you should see some cells have found one or more matches - the potential matches are show in a list in each cell. Next to each potential match there is a ‘tick’ and a ‘double tick’. To accept a reconciliation match you can use the ‘tick’ options in cells. The ‘tick’ accepts the match for the single cell, the ‘double tick’ accepts the match for all identical cells.

*   Create a text facet on the Publisher column
*   Choose ‘International Union of Crystallography’

In the Publisher column you should be able to see the various potential matches. Clicking on a match will take you to the VIAF page for that entity.

*   Click a ‘double tick’ in one of the Publisher column cells for the option “International Union of Crystallography”
*   This will accept this as a match for all cells - you should see the other options all disappear
*   Check the ‘Publisher: Judgement’ facet. This should now show that 858 items are ‘matched’ (if this does not update, try refreshing the facets)

We could do these one by one, but if we are confident with matches, there is an option to accept all:

*   Remove all filters/facets from the project so all rows display
*   In the Publisher column use the dropdown menu to choose ‘Reconcile > Actions > Match each cell to its best candidate’

There are two things that reconciliation can do for you. Firstly it gets a standard form of the name or label for the entity. Secondly it gets an ID for the entity - in this case a VIAF id. This is hidden in the default view, but can be extracted:

*   In the Publisher column use the dropdown menu to choose ‘Edit column > Add column based on this column…’
*   Give the column the name ‘VIAF ID’
*   In the GREL expression box type `cell.recon.match.id`
*   This will create a new column that contains the VIAF ID for the matched entity.

## Further reading 📚
* [Library Carpentry OpenRefine lesson](https://librarycarpentry.org/lc-open-refine/), on which this workshop is based
* OpenRefine recipes from [Library Workflow Exchange](http://www.libraryworkflowexchange.org/tag/openrefine/)
* [OpenRefine LibGuide](https://guides.library.illinois.edu/openrefine/home) from the University of Illinois. A little dated (uses version 2.8) but lots of good advice
* [A worked example of fixing problem MARC data](https://www.google.com/url?q=http://www.meanboyfriend.com/overdue_ideas/tag/fixmarc/?orderby%3Ddate%26order%3DASC&sa=D&ust=1554800126179000), 5-part series by OpenRefine wizard Owen Stephens
* [OpenRefine wiki](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki&sa=D&ust=1554800126179000), an in-depth technical manual (more helpful than the Help function!)
* [OpenRefine recipes](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki/Recipes&sa=D&ust=1554800126180000), a list of useful assorted GREL scripts
* [Google Refine Cheat Sheets](https://www.google.com/url?q=https://code4libtoronto.github.io/2018-10-12-access/GoogleRefineCheatSheets.pdf&sa=D&ust=1554800126181000), a super useful regular expression cheat sheet from code4lib Toronto
* [Using regular expressions in OpenRefine](https://gist.github.com/pmgreen/6e133c5dcde65762d29c), a primer by Peter Green
