# Cleaning Data with OpenRefine 💎 Project Guide
Welcome to the VALA Tech Camp 2019 OpenRefine project guide! Congratulations on getting through the teaching part of the workshop. Now it's time to get stuck into some dodgy data 🙂

You can download a copy of the workshop slides [here](https://github.com/lissertations/openrefine/blob/master/Final_slides.pdf). (right-click to save)

Please feel free to ask Alissa or Alexis for help. If you have a question we can't answer (and we're not experts, so it's possible) or you get stuck later on, you're welcome to tweet or email Alissa and she'll get back to you 🐦

twitter: [@lissertations](https://twitter.com/lissertations) | 
email: [hello@lissertations.net](mailto:hello@lissertations.net)

## Download your dodgy data ⬇️
You have two choices of project data: a **MARC file (converted into TSV)** and a **regular spreadsheet / CSV file**. Feel free to play around with either or both!

[MARC/TSV FILE](https://github.com/lissertations/openrefine/blob/master/doaj_data.csv) | [DOAJ/CSV FILE](https://github.com/lissertations/openrefine/blob/master/doaj_data.csv)

To get these files into OpenRefine:
1. Right-click on these links to save the files to your desktop. 
2. In [OpenRefine](http://127.0.0.1:3333/), make sure you've selected 'Create Project' and 'Get data from this computer'. Click 'Browse' to locate the file, then click 'Open', then 'Next'.
3. You shouldn't need to change anything on the next screen—ensure OpenRefine is parsing your data as 'CSV / TSV / separator-based files'. Give your project a name if you like, then click 'Create Project'.

If you don't want to try your hand at specific exercises, feel free to have a play around with the different features of OpenRefine. Remember, today we learned how to:
* Import and export data in and out of OpenRefine
* Facet, filter, cluster and edit data
* Transform data using GREL (General Refine Expression Language)
* Reconcile data against an external source (VIAF)

## MARC data as TSV (marc_data.tsv)
*Focused demonstration of how OpenRefine can be used to enhance MARC data*

This dataset comprises 1000 random MARC files from Harvard University’s catalogue, as compiled by Terry Reese, the creator of MarcEdit. (Originally on Terry's [Github repo](https://github.com/reeset/data_packs/tree/master/marc))

Ordinarily you would need to use a program like [MarcEdit](https://marcedit.reeset.net/downloads) to convert a binary MARC file into a tab-separated values (TSV) file. OpenRefine doesn't deal well with MARC files, so we need to convert it into a format that looks more like a spreadsheet. Terry Reese, the creator fo MarcEdit, has written a [workflow](https://blog.reeset.net/archives/1873) on how you can do this.

Many libraries use both MarcEdit and OpenRefine to clean and improve their data. The two programs complement each other, but some things (such as clustering) are better or easier in one program than the other—it's a bit of trial and error to determine what might work best for you.

### Things to try

#### Facet by tag
Because of the layout of a MARC file within OpenRefine, it's often easiest to look at all the instances of a given field at once, eg. all the 650 fields. Open up a text facet (Facet > Text facet) and select the field you want.

**Caution!** Even if you have faceted by tag before working with the Content column, selecting anything in the 'Edit column' menu will indeed edit the entire column, *not just the faceted data*. Ask me how I know this 🙃

Often having two facets can be a great way to spot inconsistencies in your data. For example, put a text facet on the Tags column, then another on the Indicators column. This will show you the indicator combinations for each instance of that particular field. If any shouldn't be there, simply click on the indicator values on the left to pop open a text box, where you can edit them.

#### Fix dodgy UTF-8 encoding
I'm not sure if there is any in this dataset, but you may find that moving data between various programs and systems can result in special characters going a bit off, and 'é'' becomes 'Ã©', among other things.

To rectify, simply run this transformation on the offending column:

`value.reinterpret("utf-8")` 

#### VIAF reconciliation with MARC data
As mentioned, this only works with plain text (that is, text without MARC subfields). This workflow is quite convoluted, and you don't have to finish it today! This is part of a real workflow that I used at work.

To strip the MARC subfields from the 100 field:
1. Facet by 100 field
2. If there are $e subfields in here, you'll need to split them out into their own column, to be reattached later. On the Content column, go to Edit cells > Split multi-valued cells, and use '$e' as the separator. Do not delete column, do not guess cell type
3. Use the following transformation: 
`value.replace('$a','').replace('$d',' ').replace('$q',' ').replace('$c',' ').split('$e')[0]`
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

16. (extra) Checking for errant missed full stops: text filter > \.$ [regex] > invert = this shows all fields that do NOT have a full stop at the end. Most of these will be fields with a $d but no $e. 
* Text filter > $d > invert
* Text filter > $4 > invert
* Transform `value + '.'`


*   Touch on the MarcEdit–OpenRefine export/import capabilities, link to Terry’s page + maybe a screenshot
*   Facet by field
*   Recipes for stripping subfields (esp for VIAF reco + name clustering)
*   Reconcile author names against VIAF forms
*   Recipes / procedure for reinserting subfields (esp. the need for manual reco between 100 $c and $q)
*   Sentence case GREL?
*   These dot points aren’t in order, necessarily
*   Base this chiefly on the NLA version

## DOAJ data as DSV (doaj_data.csv)
*Broader demonstration of the features and capabilities of OpenRefine for typical spreadsheets*

*   Split / join multi-valued cells
*   Transform to number / date \[as per NLA version\]
*   Reconcile publisher names with VIAF IDs (as demoed; this is complex so I’m cool with people replicating it themselves in project time)
*   Few other things

### Things to try

#### Exercise: Reconciling publisher names with VIAF IDs

## Further reading
* [Library Carpentry OpenRefine lesson](https://librarycarpentry.org/lc-open-refine/), on which this workshop is based
* OpenRefine recipes from [Library Workflow Exchange](http://www.libraryworkflowexchange.org/tag/openrefine/)
* [OpenRefine LibGuide](https://guides.library.illinois.edu/openrefine/home) from the University of Illinois. A little dated (uses version 2.8) but lots of good advice
* [A worked example of fixing problem MARC data](https://www.google.com/url?q=http://www.meanboyfriend.com/overdue_ideas/tag/fixmarc/?orderby%3Ddate%26order%3DASC&sa=D&ust=1554800126179000), 5-part series by OpenRefine wizard Owen Stephens
* [OpenRefine wiki](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki&sa=D&ust=1554800126179000), an in-depth technical manual (more helpful than the Help function!)
* [OpenRefine recipes](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki/Recipes&sa=D&ust=1554800126180000), a list of useful assorted GREL scripts
* [Google Refine Cheat Sheets](https://www.google.com/url?q=https://code4libtoronto.github.io/2018-10-12-access/GoogleRefineCheatSheets.pdf&sa=D&ust=1554800126181000), a super useful regular expression cheat sheet from code4lib Toronto
* [Using regular expressions in OpenRefine](https://gist.github.com/pmgreen/6e133c5dcde65762d29c), a primer by Peter Green
