# NLA OpenRefine Workshop Outline
Compiled and presented by Alissa McCulloch, Australian Monographs

## Background
2 ½ hour slightly condensed version of Alissa's [OpenRefine](http://openrefine.org/download) workshop to be delivered at
[VALA Tech Camp](https://www.vala.org.au/events/vala-tech-camp-2019/camp-sessions-w2-mcculloch/) in mid-April. This version removes the project time at the end and focuses the workshop's content on NLA processes and priorities.

## Workshop Objectives
By the conclusion of this workshop participants will be familiar with how to:

-   Import and export data in and out of OpenRefine
-   Facet, filter, cluster and edit data
-   Transform data using GREL
-   Reconcile data from an external source (VIAF)

## Setup
Ensure OpenRefine has been [downloaded](http://openrefine.org/download) and is working on your machine (Note to self: ask IT to deploy this on the training room computers).

### 1. What is OpenRefine and why do I want it?
How will it help me improve my metadata? Also, a brief overview of what OR is and is not good at. **It will not replace cataloguers! It will make metadata management easier and more enjoyable :)**

### 2. Getting data into OpenRefine
From a variety of sources: Excel spreadsheet, CSV, MARC file (via MarcEdit---see [this](https://blog.reeset.net/archives/1873) explainer from Terry Reese)

NB the OpenRefine export option in the menu doesn't actually work (at least not on Macs) - you'll need to use the other option, from the home menu.

### 3. Getting to know your data
How should your data look? Familiarisation with rows and columns, moving, renaming, etc. This is more important when dealing with a regular spreadsheet - when importing JSON from MarcEdit the delimitation is done for you.

### 4. Simple cleaning without code: facets and filters
Isolating and editing cells in bulk. Including how to do these things with regex (touch on this briefly). This specifies 'without code' in case some attendees are apprehensive about code. There is code later, but this might help people feel like they can Do The Thing without needing to code

### 5. Clustering and editing
AKA the killer feature of OpenRefine. Let the algorithm assist you to normalise your messy data---it will highlight cells that look slightly different but probably mean the same thing and group them together, ready for you to decide which form you want.

### 6. Transforms
In OpenRefine, 'transform' is a noun. Overview of 'common transforms' (there's a menu for these: remove whitespace, upper/lowercase, to null, etc) and less-common transforms (using GREL, or General Refine Expression Language, which can appear scary but is quite a friendly scripting language. Include plenty of examples and some links to canned GREL scripts for common tasks)

### 7. Reconciliation
AKA the other killer feature: matching your data against an external source, in this case reconciling name headings against VIAF. (Make sure to include the http://refine.codefork.com/ link + instructions)

### 8. Getting data out of OpenRefine
Exporting projects, exporting data (these are not the same thing!). Including a recap of getting it back into MarcEdit
