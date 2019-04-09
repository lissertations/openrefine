# Cleaning Data with OpenRefine 💎 Project Guide

Welcome to the VALA Tech Camp 2019 OpenRefine project guide! Congratulations on getting through the teaching part of the workshop. Now it's time to get stuck into some dodgy data 🙂

You have two choices of project data: a **MARC file (converted into TSV)** and a **regular spreadsheet / CSV file**. Feel free to play around with either or both!

Please feel free to ask Alissa or Alexis for help. If you have a question we can't answer (and we're not experts, so it's possible), you're welcome to tweet or email Alissa and she'll get back to you 🐦

twitter: [@lissertations](https://twitter.com/lissertations)
email: [hello@lissertations.net](mailto:hello@lissertations.net)
project guide: lissertations.github.io/openrefine

## MARC data as TSV (marc_data.tsv)
*Focused demonstration of how OpenRefine can be used to enhance MARC data*

This dataset comprises 1000 random MARC files from Harvard University’s catalogue, as compiled by Terry Reese, the creator of MarcEdit. 

Click [here](https://github.com/lissertations/openrefine/blob/master/marc_data.tsv) to download the dataset. (Originally on Terry's [Github repo](https://github.com/reeset/data_packs/tree/master/marc))

The MARC data has been exported into a TSV (tab-separated values) file for you. OpenRefine doesn’t deal well with binary MARC files (it tries to convert them to MARCXML instead, which we don't want), but it does deal well with files that behave like spreadsheets. To get around this, you need to convert your MARC file into a format that behaves like a spreadsheet, and TSV is one of these formats. You can use a program like MarcEdit to do this. The two programs complement each other, and Terry has written a [workflow](https://blog.reeset.net/archives/1873) to help you move data from MarcEdit to OpenRefine.

### Things to remember when editing a MARC/TSV file in OpenRefine
* **Facet by tag.** Because of the layout of a MARC file within OpenRefine

*   Touch on the MarcEdit–OpenRefine export/import capabilities, link to Terry’s page + maybe a screenshot
*   Facet by tag
*   Recipes for stripping subfields (esp for VIAF reco + name clustering)
*   Reconcile author names against VIAF forms
*   Recipes / procedure for reinserting subfields (esp. the need for manual reco between 100 $c and $q)
*   Sentence case GREL?
*   These dot points aren’t in order, necessarily
*   Base this chiefly on the NLA version
*   Add the UTF 8 thing There is a UTF-8 thing we can use omg omg. DOnt put it in this slide, but maybe the project guide, or somewhere?? [https://github.com/OpenRefine/OpenRefine/wiki/Recipes#a%C3%83n-t%C3%83muchent-----a%C3%AFn-t%C3%A9muchent](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki/Recipes%23a%25C3%2583n-t%25C3%2583muchent-----a%25C3%25AFn-t%25C3%25A9muchent&sa=D&ust=1554800126164000)

[https://gist.github.com/pmgreen/6e133c5dcde65762d29c](https://www.google.com/url?q=https://gist.github.com/pmgreen/6e133c5dcde65762d29c&sa=D&ust=1554800126165000)

## DOAJ data as DSV (doaj_data.csv)
*Broader demonstration of the features and capabilities of OpenRefine for typical spreadsheets*

*   Split / join multi-valued cells
*   Transform to number / date \[as per NLA version\]
*   Reconcile publisher names with VIAF IDs (as demoed; this is complex so I’m cool with people replicating it themselves in project time)
*   Few other things

### Exercise: Reconciling publisher names with VIAF IDs

In this exercise you are going to use the VIAF Reconciliation service written by[Jeff Chiu](https://www.google.com/url?q=https://twitter.com/absolutelyjeff&sa=D&ust=1554800126168000). Jeff offers two ways of using the reconciliation service - either via a public service he runs at[http://refine.codefork.com/](https://www.google.com/url?q=http://refine.codefork.com/&sa=D&ust=1554800126169000), or by installing and running the service locally using the instructions at[https://github.com/codeforkjeff/refine\_viaf](https://www.google.com/url?q=https://github.com/codeforkjeff/refine_viaf&sa=D&ust=1554800126170000).

If you are going to do a lot of reconciliation, please install and run your own local reconciliation service - the instructions at[https://github.com/codeforkjeff/refine\_viaf](https://www.google.com/url?q=https://github.com/codeforkjeff/refine_viaf&sa=D&ust=1554800126171000) make this reasonably straightforward.

Once you have chosen which service you are going to use:

*   In the Publisher column use the dropdown menu to choose ‘Reconcile->Start Reconciling’
*   If this is the first time you’ve used this particular reconciliation service, you’ll need to add the details of the service now

*   Click ‘Add Standard Service…’ and in the dialogue that appears enter:

*   “http://refine.codefork.com/reconcile/viaf” for Jeff’s public service
*   “http://localhost:8080/reconcile/viaf” if you are running the service locally

*   You should now see a heading in the list on the left hand side of the Reconciliation dialogue called “VIAF Reconciliation Service”
*   Click on this to choose to use this reconciliation service
*   In the middle box in the reconciliation dialogue you may get asked what type of ‘entity’ you want to reconcile to - that is, what type of thing are you looking for. The list will vary depending on what reconciliation service you are using.

*   In this case choose “Corporate Name” (it seems like the VIAF Reconciliation Service is slightly intelligent about this and will only offer options that are relevant)

*   In the box on the righthand side of the reconciliation dialogue you can choose if other columns are used to help the reconciliation service make a match - however it is sometimes hard to tell what use (if any) the reconciliation service makes of these additional columns
*   At the bottom of the reconciliation dialogue there is the option to “Auto-match candidates with high confidence”. This can be a time saver, but in this case you are going to uncheck it, so you can see the results before a match is made
*   Now click ‘Start Reconciling’

Reconciliation is an operation that can take a little time if you have many values to look up. However, in this case there are only 6 publishers to check, so it should work quite quickly.

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
*   In the Publisher column use the dropdown menu to choose ‘Reconcile->Actions->Match each cell to its best candidate’

There are two things that reconciliation can do for you. Firstly it gets a standard form of the name or label for the entity. Secondly it gets an ID for the entity - in this case a VIAF id. This is hidden in the default view, but can be extracted:

*   In the Publisher column use the dropdown menu to choose ‘Edit column->Add column based on this column…’
*   Give the column the name ‘VIAF ID’
*   In the GREL expression box type cell.recon.match.id
*   This will create a new column that contains the VIAF ID for the matched entity

## Further reading
* [Library Carpentry lessons, on which this workshop is based](https://librarycarpentry.org/lc-open-refine/)
* [A worked example of fixing problem MARC data](https://www.google.com/url?q=http://www.meanboyfriend.com/overdue_ideas/tag/fixmarc/?orderby%3Ddate%26order%3DASC&sa=D&ust=1554800126179000), 5-part series by OpenRefine wizard Owen Stephens
* [OpenRefine wiki](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki&sa=D&ust=1554800126179000), an in-depth technical manual
* [OpenRefine recipes](https://www.google.com/url?q=https://github.com/OpenRefine/OpenRefine/wiki/Recipes&sa=D&ust=1554800126180000), a list of useful assorted GREL scripts
* [Google Refine Cheat Sheets](https://www.google.com/url?q=https://code4libtoronto.github.io/2018-10-12-access/GoogleRefineCheatSheets.pdf&sa=D&ust=1554800126181000), super useful quick reference courtesy code4lib Toronto
