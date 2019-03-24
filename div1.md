# NLA OpenRefine Workshop Outline
Compiled and presented by Alissa McCulloch, Australian Monographs

## Background
This workshop is a slightly condensed 2 ½ hour version of Alissa's [OpenRefine](http://openrefine.org/download) workshop to be delivered at
[VALA Tech Camp](https://www.vala.org.au/events/vala-tech-camp-2019/camp-sessions-w2-mcculloch/) in mid-April. This version focuses the workshop's content on NLA processes and priorities—in particular, using OpenRefine to enhance MARC records.

## Workshop Objectives
By the conclusion of this workshop participants will be familiar with how to:

-   Import and export data in and out of OpenRefine
-   Facet, filter, cluster and edit data
-   Transform data using GREL
-   Reconcile data from an external source (VIAF)

## Setup
Ensure OpenRefine has been [downloaded](http://openrefine.org/download) and is working on your machine (Note to self: ask IT to deploy this on the training room computers).

Double-click on the OpenRefine icon (the diamond) and after a few seconds the application should open in your web browser. If it doesn't open, go to http://127.0.0.1:3333

### 1. What is OpenRefine and why do I want it?
OpenRefine (formerly Google Refine) is a free and open-source software program for cleaning and editing messy data at scale. Instead of painstakingly editing data fields one at a time, OpenRefine provides some powerful tools to help you clean many thousands of fields at once. These include advanced filtering and faceting abilities, a 'cluster and edit' tool that algorithmically sorts and cleans your data, and a reconciliation function to compare your data with an external source.

**OpenRefine, and tools like it, cannot and will not replace cataloguers.** It can help make cataloguing and metadata management faster, easier and more enjoyable. It can also enable tasks like authority control to be undertaken at far greater scale (and, in my view, with far greater accuracy) than is currently being done at NLA. But there will always be a need for skilled cataloguers to do the work itself. (That's where you come in!)

### 2. Getting data into OpenRefine
From a variety of sources: Excel spreadsheet, CSV, MARC file (via MarcEdit---see [this](https://blog.reeset.net/archives/1873) explainer from Terry Reese)

OpenRefine claims to be able to work with MARC data out of the box, but it's very clunky and requires a *lot* of mucking around with columns. Using MarcEdit to convert a file from binary MARC (.mrc) to tab-separated values (.tsv) is significantly easier.

NB the OpenRefine export option in the MarcEditor menu doesn't actually work (at least not on Macs) - you'll need to use the other option from the home menu, or from File > Export Data To > OpenRefine.

### 3. Getting to know your data
How should your data look? Familiarisation with rows and columns, moving, renaming, etc. This is more important when dealing with a regular spreadsheet - when importing TSV from MarcEdit the delimitation is done for you.

### 4. Simple cleaning without code: facets and filters
Isolating and editing cells in bulk. Including how to do these things with regex (touch on this briefly). This specifies 'without code' in case some attendees are apprehensive about code. There is code later, but this might help people feel like they can Do The Thing without needing to code :)

### 5. Clustering and editing
AKA the killer feature of OpenRefine. Let the algorithm assist you to normalise your messy data---it will highlight cells that look slightly different but probably mean the same thing and group them together, ready for you to decide which form you want.

Demonstrate this with the DET CRC data--despite around 1/3 of the dataset having been catalogued by the time I got my hands on it, there were still inconsistencies in the name headings that OpenRefine was very good at sorting out. (It took a couple of hours, though.)

### 6. Transforms
In OpenRefine, 'transform' is a noun. Overview of 'common transforms' (there's a menu for these: remove whitespace, upper/lowercase, to null, etc) and less-common transforms (using GREL, or General Refine Expression Language, which can appear scary but is quite a friendly scripting language. Include plenty of examples and some links to canned GREL scripts for common tasks)

### 7. Reconciliation
AKA the other killer feature: matching your data against an external source, in this case reconciling name headings against VIAF. (Make sure to include the http://refine.codefork.com/ link + instructions)

#### In the MARC File
1. Facet by 100 and 700 fields
2. Split cells in Content column into several columns by separator [$e]. Do not delete column, do not guess cell type
3. Text transform on Content column `value.replace('$a','').replace('$d',' ').replace('$q',' ').replace('$c',' ').split('$e')[0]`
4. Reconcile Content column against VIAF People/persons target
5. Facet by 245 fields too (so you know what you're looking at)


### 8. Getting data out of OpenRefine
Exporting projects, exporting data (these are not the same thing!).

Including a recap of getting it back into MarcEdit (export as TSV, > MarcEdit > import data from OpenRefine)
