BUILDING LAB DOCUMENTATION
--------------------------

The current directory in the notes below is assumed to be the directory containing
this file - lab-instructions.


1. BUILD DOCS
- - - - - - -

To build the lab-docs, make sure you have the Ant build tool installed.

Open a shell/command window and change into the build directory and run "ant".

The default Ant target will generate HTML and PDF versions of the lab docs in the
following locations:

HTML: build/target/generated-lab-docs/docs/lab/*.html - start with index.html
 PDF: build/target/generated-lab-docs/docs/lab.pdf

Build targets are:

  lab.doc        make everything
  lab.doc.html   make HTML files
  html           make HTML files
  lab.doc.pdf    make lab.pdf
  pdf            make lab.pdf

To add/remove a section you need to modify two files:

  build/doc-targets.xml - add/remove the flat-copy in target "lab.doc.prep"
  build/lab/lab.xml - add/remove the entity definition

 
2. HOW IT WORKS
- - - - - - - -

The documentation is written using an XML syntax called DocBook.  DocBook concentrates
on content rather than presentation - so don't get hung up trying to format the
material - DocBook is deliberately designed not to let you.  Instead, it uses XSL
stylesheets to convert the XML to either HTML or PDF output.  All the presentation
control is in the XSL stylesheets and what they generate, allowing DocBook to
generate output that looks good as both PDF and HTML.

There are a number of ways to use DocBook.  We are building our documentation using
a simple DocBook generation framework (DBF) from the Apache Velocity project - see
http://velocity.apache.org/docbook for more info (but it's pretty minimal).  The DBF
framework is in build/lib and should not be changed.

Because DBF is very simple, it doesn't offer much customisation control, however there
is enough.

Customisation is controlled by the contents of the lab/ subdirectory.  DO NOT change
files in the lib/ subdirectory (you will find the same files in lib as in lab and it
is easy to get confused).

 * build/lib/ contains the original Docbook source - DO NOT CHANGE
 * build/lab/ holds our versions of the same files, configured for this documentation.

The interesting files in build/lab ares:

css/html/html.css        The cascading stylesheet (CSS) for the generated HTML.
images                   Images used throughout the docs, such as the note and tip icons.
lab.xml                  Master configuration file - defines the contents of the docs.
styles/html/custom.xsl   Custom stylesheet for HTML generation.
styles/pdf/custom.xsl    Custom stylesheet for PDF generation.
styles/pdf/titlepage.xml Defines contents of PDF title page.

Most importantly, to control which labs appear in the generated docs and in which order,
edit lab.xml.

Just to be clear: to add or remove a lab, you need to modify TWO files:

build/doc-targets.xml    Defines which files are copied to target for building.
lab/lab.xml              Defines the content of the documentation.

These two files must be kept in sync - lab.xml cannot refer to a file that was not
copied by doc-targets.xml.

You will find some notes on using Docbook in build/DOCBOOK=HELP.txt.


3. CUSTOMISING OUTPUT
- - - - - - - - - - -

Unless you really know XSL, don't bother reading any further!

All the stylesheets are part of the DocBook distribution in

   build/lib/docbook/src/zip/docbook-xsl-1.70.0.zip

If you unpack this zip there are two main subdirectories of interest:

html - contains stylesheets used to generate HTML
fo   - contains stylesheets used to generate FOP (which in turn produces PDF) 

Customisation is essentially the following:

1. Find the stylesheet that contains what you want to customise - this is the hardest
   part as the stylesheets are not trivial.

2. Copy the complete <xsl:template> into the custom.xsl file for HTML or PDF (either
   styles/html/custom.xsl or styles/pdf/custom.xsl).
   
3. Modify the copy in custom.xsl and it will be used instead of the default when you
   next build.

A much simpler way to customise output for HTML may be to edit css/html/html.css and
tweak the styles instead.  Most elements in the generated HTML have associated classes,
many of which are not even defined in html.css.  If using Firefox or Chrome, the Firebug
add-on is recommended for viewing styles in a page.

DO NOT modify the docbook distribution and rezip it - it will be lost next time we
move to a newer version of docbook.

31 May 2013

