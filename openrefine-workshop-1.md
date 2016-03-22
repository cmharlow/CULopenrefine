# Data Cleaning: an Introduction to OpenRefine
Got messy data? OpenRefine is a free and powerful tool for exploring, normalizing, and manipulating tabular data. This introductory workshop will teach you the basics of OpenRefine, including how to import data, discover and clean up inconsistent values, reformat dates, and expand values into multiple columns.

## Why OpenRefine?

* Free, open-source software
* Runs on Windows, Mac, and Linux
* Explore data through faceting and scatterplots
* Normalize data by clustering similar text values
* Track all changes to the data, with full undo/redo
* Apply a series of clean-up steps to other, similar similar files
* Merge data from different tables
* Enhance or reconcile your data with external web services

This workshop is based upon two existing OpenRefine tutorials:
* http://enipedia.tudelft.nl/wiki/OpenRefine_Tutorial
* http://emudrak.github.io/2015-01-15-cornell/lessons/OpenRefine/open-refine-demo.html


## Download the workshop data

For the current tutorial, we will use slightly cleaner subset of the university data used in those tutorials, just to avoid unnecessary distractions caused by the extreme messiness of the original dataset.  (Don't worry, there is still plenty of messy data to be cleaned up!)

1. Download the workshop data.  Right-click the following link and "Save link as..." to your computer:

  [universities.csv](https://gist.githubusercontent.com/kgjenkins/99f649989e61cd2cc7b3/raw/13758f4901bb2186cdd52f031b958337f5df94fa/universities.csv)


## Download OpenRefine

If you don't already have OpenRefine on your computer, download it from the OpenRefine website:

1. Go to http://openrefine.org/download.html

2. Download version 2.5 for your operating system (at this time, 2.6 is still beta, and has some issues)

3. For Windows, we need to unzip the .zip download package -- right-click > 7-Zip > Extract here


## Run OpenRefine and explore the data

1. Run google-refine.exe

  Yes, OpenRefine was a Google project for a while, but is now independent, and version 2.5 still has some Google branding.  It is a Java program that runs on your computer, but is viewed through a browser.  However, no Internet connection is required, and all your data remains on your local computer.

  After clicking the .exe, your default web browser will open the following page in a new tab:

  `http://127.0.0.1:3333/`

  This is a local address on your computer that points to OpenRefine.  OpenRefine does not currently work in Internet Explorer, so you may need to open a different browser (like Chrome) and go to that URL.

2. Create a new project by clicking the "Choose Files" button, and browsing to the universities.csv file on your computer.

3. Click "next", and look at the preview of the data.  Numeric values should appear in green.  (If not, check the "Parse cell text into numbers, dates, ..." box at the bottom right.  For some datasets, you may need to adjust other settings.)

4. If the preview looks okay, click the "Create Project" button at the top right.

  Note that OpenRefine will not modify the original data file, so any original data stays safe.  As you clean up and modify the data, any changes will be automatically preserved within OpenRefine (so if you close and reopen the program, you can continue where you left off), and you can also export the changes to a new file.

  Once the data is loaded, we should see the first 10 rows (of 1072 total rows).

5. Change the view to show 50 rows.

  Take a look at the data, and notice that their are many problems, including:

  * Oddly encoded university names, like "%C3%89cole Polytechnique de Montr%C3%A9al"
  * Inconsistent endownment values: some are numbers (green), others are text (black) like "US $ 120 million"
  * Inconsistent country names, like "United States", "USA"


## Faceting

Let's start by fixing the country names.  Rather than scanning through every row looks for inconsistencies, we can use faceting to explore the values.

1. Click the down-arrow next to the "country" column > Facet > Text facet

  The country facet box will appear to the left, containing each distinct country value that appears in the dataset, along with the number of times it appears.  Notice that there are 55 occurrences of Canada.  We can immediately see that there are also some "Canada" values that have extraneous text at the end.

2. Hover over the "Canada B1P 6L2" value and click the "edit" link.

3. Delete everything after the final "a" in "Canada" (including the space) and click "Apply".

  Notice that there are now 56 occurrences of Canada.

4. Fix the other "Canada C1A 4P3..." value.


## Clustering

Scroll down to the bottom of the country facet, and notice that there are several forms of "U.S.", "United States", etc.  OpenRefine comes with several clustering algorithms that can be used to group similar values, and values in these groups can be edited all at the same time.

1. Click the "Cluster" button at the top right of the country facet box.

  We should see three different clusters.  These were identified using the fingerprint key collision method.  (Don't worry about what precisely that means right now -- the point is that OpenRefine has found some similar values, and we now get to decide if they should be merged or now.)

2. Click the "Merge?" checkbox for each and set the New Cell Value to "United States" for each one.

3. Click the "Merge Select & Re-Cluster" button.

  No other clusters are found using fingerprint key collision, so let's try a different method.

4. Set the following:
  * "Method" = "nearest neighbor"
  * "Distance Function" = "PPM"
  * "Radius" = 2

  We should see two clusters.

5. Merge these cluster to "Netherlands" and "England".

  No other clusters are found using the current settings, so let's check for even fuzzier matches.

6. Set "Radius" = 3

  You can think of a larger radius as creating a larger cluster, including values that are even further apart.  Notice that this finds some false matches, like "Netherlands" and "Switzerland" -- don't merge those!  However, we should merge "United States" and "United States of America".  If you would like to learn more about the cluster algorithms, click the [Find out more...](http://code.google.com/p/google-refine/wiki/Clustering) link at the top of the clustering window.

7. Close the clustering window.


## Undo

If you make a mistake, you can always undo a step, or even a whole series of steps, all the way back to the beginning when you opened the original data file.  OpenRefine keeps a log of all the changes, and we can look at the state of the data as it existed at any previous step.

1. Click the "Undo/Redo" link at the top left, above the facets.

  To back up to a previous step, click on the last step you would like to retain.  You will be able to view the data as it existed at that point.  **Caution: all steps following the selected step will be lost if you start editing again at that point.**  So if you change you mind and want to keep all the steps, be sure to click the very last step in the list.


## Deleting rows

You may have noticed the country name "Utopia", which doesn't really exist (yet).  Let's use a facet filter to limit our view of the data table.

1. Click "Utopia" in the list of country facet values.

  "Utopia" is now listed in orange letters to indicate that the data table is now being filtered to those rows where the country equals "Utopia".  At this point, there should be just 1 matching row.  The corresponding university name is "WikiProject Universities", with an endowment of $123,456,789.  Since this is fake data, let's delete this row from our dataset.

2. In the leftmost column of the table, click the down-arrow next to "All" > Edit rows > Remove all matching rows

  There will now be no data showing, since there are now 0 matching rows.  Let's remove the facet filter:

3. Click the orange "Utopia" in the list of country facet values.

  It should now disappear from the facet value list (since it no longer exists in the data).  Let's now remove the country facet:

4. Click "x" at the top left of the country facet box.

  We should now be viewing the first page of all 1071 rows.


## Faceting numeric values

Look at the "numStudents" column, the rightmost column in the table.  Notice that there are some numbers (in green), some blank cells, and also some text values like "3000+" and "~2300".  We can use a numeric facet to help clean up these values.

1. Click the down-arrow next to the "numStudents" column > Facet > Numeric facet

  The numStudents facet box will appear to the left, containing a histogram of numeric values, plus some checkboxes for filtering to numeric, non-numeric, blank, or error.  Currently, the histogram is not very useful due to some dubious values like 4.2 billion students, so let's try to fix those first.

2. Check the "Numeric" checkbox, and uncheck the others.

3. Click the down-arrow next to the "numStudents" column > Sort...

4. Sort cell values as numbers, largest first.

  The first 6 rows have unbelievably large numbers of students.  (The 7th, San Diego Community College District really does claim to have around 130,000 students on their webiste.)

5. Hover over the cell containing "4197033329" and click "edit".

6. Press backspace to delete the value, then click "Apply".

7. Repeat for the next five rows.

8. Click the "Refresh" button above the numStudents facet box.

  Now the histogram is a bit more useful.  Try moving the sliders at each side of the histogram to see how they can be used to filter the results.


## Text transforms

Let's now take a look at the non-numeric (text) values.

1. Uncheck the "Numeric" checkbox, and check the "Non-numeric" box.

  We should see 18 matching rows.  While we could edit each one manually, this is a good opportunity to learn about the power of text transforms, which allow us to write a simple (or complex!) formula to modify many different text values all at once.

2. Click the down-arrow next to the "numStudents" column > Edit cells > Transform...

3. Enter the following expression:

  `value.replace("+", "")`

  This expression says to take the value of the cell, and replace any "+" characters with nothing.  Look at the preview table below the expression, and notice how it shows the original values alongside the modified values, after applying the expression.  So far, our expression cleans up the "+" at the end of values, but not the "~" at the beginning.  So let's expand our expression:

  `value.replace("+", "").replace("~", "")`

4. Click "OK" to apply the expression to all the matching rows.

  There is still one problem cell "faculty to student ratio: 12:1".

5. Hover over that cell, click edit, and delete the value.

  Now the values all look like numbers -- but are they?  No, they are still text, which appears in black instead of the numeric green.

6. Click the down-arrow next to the "numStudents" column > Edit cells > Common transforms > To number

  If all goes well, we should now have 0 matching rows, because we are still filtering to show only the non-numeric rows, and there are none now.

7. Click "x" at the top left of the numStudents facet box to remove the facet.

  If OpenRefine hangs -- press the the "Esc" key and then click the "Sort" link above the table > Remove sort.  This seems to be a problem when there is a sort on a column with mixed numeric and non-numeric values.


## More text transforms

You may have noticed strange characters in some of the university names, such as "%C3%89cole Polytechnique de Montr%C3%A9al".  This is an artifact of URL encoding, which is a method by which accents and other characters can be safely included in a URL.  However, it's not what we want to see in our table, so we will use a special text transform to fix these values.

1. Click the down-arrow next to the "university" column > Edit cells > Transform...

2. Enter the following expression:

  `value.unescape('url')`

3. Check the values in the preview, then click "OK".

  Notice the yellow alert at the top of the window reports that 89 cells were transformed.  That saved a lot of manual work!  For a list of other text functions you can use in OpenRefine, see the documentation for GREL String Functions:

  https://github.com/OpenRefine/OpenRefine/wiki/GREL-String-Functions


## Even more transforms for really messy data

The "endowment" column is a real mess, containing an unruly mix of numeric values (assumed to be US dollars) and text values representing millions or billions of US or Canadian dollars and other currencies, as well as non-data values like "No available figures".  (Yes, this dataset is a real mess!)  We will try to clean up these values using several transforms.

First, let's focus on the non-numeric values.

1. Click the down-arrow next to the "endowment" column > Facet > Numeric facet

2. Check the "Non-numeric" checkbox, and uncheck the others.

  Since there are instances of "Million" with uppercase and lowercase "M", it will be easier to write our transforms if we convert everything to lowercase (or uppercase, as long as we are consistent) -- any numbers won't be affected.

3. Click the down-arrow next to the "endowment" column > Edit cells > Common transforms > To lowercase

  Since we are assuming US dollars for the numeric values, let's strip out any text indicating US dollars.

4. Click the down-arrow next to the "endowment" column > Edit cells > Transform...

5. Enter the following expression and look at the preview:

  `value.replace(/us ?d?/, "")`

  This expression is a bit different than what we used before.  Notice that the text being replaced is surrounded by "/" slashes instead of double quotes.  The slashes contain what is called a "regular expression", where certain characters have magical values.  Regular expressions are a part of most programming languages, and are extremely powerful tools for matching patterns in text.

  In the regular expression above, the question mark after the space and the "d" means that those characters are optional, so that expression above is a much shorter way of writing:

  `value.replace("us", "").replace("us ", "").replace("usd", "").replace("us d", "")`

  Before clicking "OK", let's improve our expression even more.

6. Enter the following expression:

  `value.replace(/u\.?s\.? ?d?/, "").replace(/[$,]/, "")`

  In regular expressions, the period "." is a wildcard (matching any character) so to match a literal period we have to escape it by putting a backslash "\" before it.  In the second replace, the square brackets say to match any of the characters they contain, so it effectively removes every "$" or "," character.

7. Click OK.


## Converting millions and billions to zeros

Next, we'll apply some magic to turn "million" and "billion" into the correct number of zeros.  We can't just replace "million" by "000000", because a value like "12.3 million" would end up as "12.3000000".  The solution is to remove the "million" text, convert the digits to a number type, and then multiply by 1000000.  Of course, we only want to do this when the cell text contains "million", so first we will filter to just those rows.

1. Click the down-arrow next to the "endowment" column > Facet > Custom text facet

2. Enter the following expression:

  `value.contains("million")`

3. In the facet box that appears, click to select just the "true" rows. (There should be 253 rows containing "million".)

4. Click the down-arrow next to the "endowment" column > Edit cells > Transform...

5. Enter the following expression:

  `value.replace("million", "").toNumber()*1000000`

6. Remove the true/false facet we created for "million", and repeat the steps above for "billion".  (Be sure to multiply by 1000000000.)

  At this point, most of the remaining non-numeric values are either foreign currency amounts, or errors.  For now, let's simply delete all the rows that do not have numeric data.

7. Remove all facets except for the "endowment" numeric facet (add it back if you need to).

8. Select the "non-numeric" and "blank" values.

9. In the leftmost column of the table, click the down-arrow next to "All" > Edit rows > Remove all matching rows

10. Remove the "endowment" facet to view all rows.


## Exploring data with scatter plots

A scatterplot allows you to visualize the relationship between two numeric columns.  As an example, let's look at endowment versus the number of students at each school.

1. Click the down-arrow next to the "endowment" column > Facet > Scatterplot facet

  You'll see series of scatterplots made from all pairs of numeric columns in the dataset, with the "endowment" scatterplots highlighted.  Because there is such a large range of endowment values, it will help to use a logarithmic scale.

2. Click the "log" button at the bottom left of the scatterplot window.

  Now you can see some trends within the different scatterplots.  The top row, which should be highlighted, shows the relationship between "endowment" and each of the other numeric columns.

3. Click the top-right scatterplot (endowment vs. numStudents)

  Now that scatterplot appears on the left as a facet.  It's often helpful to increase the point size to see the scatterplot more clearly:

4. Click the largest of the three available point sizes.

  In the scatterplot, you can click and drag to select a rectangle of points to filter to.  For example, you might select the few points found at the bottom-right of the scatterplot to view schools with high endowments but low numbers of students.  This can be be a useful way of identifying outliers or possible errors in a dataset.


## Exporting the cleaned-up data

As mentioned earlier, OpenRefine will not modify the original data file, so any original data stays safe.  As you clean up and modify the data, any changes will be automatically preserved within OpenRefine (so if you close and reopen the program, you can continue where you left off).  But to use the cleaned data in another program, you must export your changes to a new file:

1. Click the "Export" button at the top right.

2. Choose a file format.

  We generally recommend saving your data table as a comma-separated (.csv) or tab-separated (.tsv) file, because these formats are widely compatible with other software.

3. Save the file on your computer.

  If you would like to preserve your entire OpenRefine project, including the data and all the steps that you performed, select "Export project" from the Export menu.  This saves the project as a .tar.gz file that can be directly opened in OpenRefine.  This is especially useful if you are working on a public computer and want a copy of the project that you can continue working later on another computer.  Use the "Import Project" option to open the .tar.gz project file.

---

This tutorial was written by Keith Jenkins, kgj2@cornell.edu for a workshop held 2016-03-15 at Cornell University's Mann Library.

For more information about OpenRefine and how to use it, see:

* http://openrefine.org/documentation.html

For an interactive way to explore the magic of regular expressions, see:

* http://regexr.com/
