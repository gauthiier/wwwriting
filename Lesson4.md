
# (Text/Document) Processor

A document processor is a piece of software that interprets text and transcode its content from one form to another. There are many types of processors, yet for the purpose of this lesson we focus on only two types: (1) __Interpreter__ and (2) __Converter__. An interpreter is very similar to your web browser: it gets as input an html file (and related scripts), interprets their syntax and content and display the results on the screen. More generally, an interpreter first parses the content of an input file, then creates its own representation of it and finally performs the commands it has interpreted. In the process, there is a translation stage where the input syntax and content is read (parsed) and transformed into a transitional or median representation. This median representation is then linked to specific commands that are in turn executed locally on the interpreters machine. In the case of the converter, such median representation get transcoded into a specified format with a specific syntax written in a file. For example, an HTML file can be converted into both a PDF _and/or_ a DOC file. Interpreters and converters are ubiquitous systems which are fundamental for both publishing and archiving practices.

### Goals

Assuming that you have a basic knowledge of the Markdown language, in this lesson you will learn how to convert a text file written in markdown to both an HTML and EPUB3 output. In order to do so, you need to instal  [Pandoc](http://johnmacfarlane.net/pandoc/) on your system as we will be using this specific markdown converter. 

The goals of this lesson are:

1. Acquire the basic understanding of how to employ pandoc to produce both HTML and EPUB output. 
2. Acquire the basic knoeledge of how pandoc can be tailored to produce other types of output.

### How

In order to complete this lesson, you should be familiar with how to operated your computer's CLI ([Lesson2](Lesson2.html)) and capable of writing markdown valid text ([Lesson3](Lesson3.html)). 

The following exercises assumes that you have markdown input file ready to convert to both HTML and EPUB. If you don't already, simply use this file ([Lesson4.md](Lesson4.md)).

To make sure that your system is set up properly, open a CLI window and type the following

	pandoc --version

If you get an error message, this means that you did not install pandoc properly. If so please visit [Pandoc](http://johnmacfarlane.net/pandoc/) and install the required software. If not you are ready to go!

Now since pandoc is a document converter its most basic form of usage consists of an _input_ file and an _output_ file. 

A command like

	pandoc -s Lesson4.md -o Lesson4.html

will produce an _output_ file (-o Lesson4.html) from the _input_ file (_Lesson4.md_). Here the argument -s will force pandoc to produce an entire valid document rather than a snippet of it.

More formally, the former command could be 

	pandoc -s -f markdown Lesson4.md -t html5 -o Lesson4.html

meaning the input file (Lesson4.md) is of type "markdown" and the output file (-o Lesson4.html) is of type "html5". You can basically omit these details if you want, markdown can recognise valid input files while the extension of your output file (here .html) will inform the program on the type of output expected. However, since pandoc has multiple possible output files to choose from, some of them of the same type but of different version (for example html and html5), it is a good practice to specify the type of output. 

Now let's simply change the output to the Rich Text Format (rtf)

	pandoc -s -f markdown Lesson4.md -t rtf -o Lesson4.rtf

or in a more compact version 

	pandoc -s Lesson4.md -o Lesson4.rtf

It is indeed very straight forward change output formats. As explained in the beginning of this Lesson, pandoc is a converter which can produce various outputs out of its internal median representation of a parsed input. 

More precisely, pandoc can read: 

> markdown and (subsets of) Textile, reStructuredText, HTML, LaTeX, MediaWiki markup, TWiki markup, Haddock markup, OPML, Emacs Org-mode, DocBook, txt2tags, EPUB and Word docx

And from these documents it can write: 

>plain text, markdown, reStructuredText, XHTML, HTML 5, LaTeX (including beamer slide shows), ConTeXt, RTF, OPML, DocBook, OpenDocument, ODT, Word docx, GNU Texinfo, MediaWiki markup, DokuWiki markup, Haddock markup, EPUB (v2 or v3), FictionBook2, Textile, groff man pages, Emacs Org-Mode, AsciiDoc, InDesign ICML, and Slidy, Slideous, DZSlides, reveal.js or S5 HTML slide shows. It can also produce PDF output on systems where LaTeX is installed.

For the purpose of this lesson we will focus on HTML(5) and EPUB(3).

#### HTML(5)

Now that we have been experiencing pandoc a little already, we will proceed by directly discussing the pandoc command used to generate this site and explain the options that we have set. To generate this html5 web page issue the following command

	pandoc -s Lesson4.md --template=style/template.html5 -c style/html5.css --bibliography=wwwrite.bib -o Lesson4.html

Now refresh your web browser and see if something changed. Perhaps not (yet). You may not realise it but you have (re)generated the entire HTML in a single pandoc command!

As you can see there is a little more arguments in the above pandoc than previously exposed in the first section. 

1. As expected we have

		-s Lesson4.md  -o Lesson4.html

	which specifies the markdown _input_ (Lesson4.md) and HTML _output_ (Lesson4.html)

2. There is a few extra options 

		--template=style/template.html5 -c style/html5.css --bibliography=wwwrite.bib

	which are instructing pandoc to (1) use the HTML5 template "style/template.html5", (2) use the [stylesheet](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) "style/html5.css" and (3) use the bibliographical information in "wwwrite.bib" if in-text referencing is needed.

Without going in too much details (since this is the topic of [Lesson6](/)), the current site employs custom templates and stylesheets (in the directory ["style/"](style/)). In these file you will find all the markup code used to (automatically) generate the layout this web page in HTML and [CSS]((https://en.wikipedia.org/wiki/Cascading_Style_Sheets)) formats. In a nutshell, on the one hand a template is responsible to instruct how to convert pandoc's median representation into an HTML format while on the other a CSS stylesheet is responsible for the layout and (design) parametrisation of the content of this HTML. Hence, rather than specifying a default pandoc output, we use our own custom templates to generate our own tailored HTML outputs.

#### EPUB(3)

It is worth mentioning at this stage that in previous example, pandoc is converting files written in one markup language "markdown" to another markup language "HTML5". At its core, EPUB is yet another markup language. Both HTML and EPUB are standard markups that are both interpreted by a web browser and epub reader respectively.

Now here is the pandoc command to generate an epub(3) of the whole site

	pandoc -S index.md Lesson1.md Lesson2.md Lesson3.md metadata.epub3.yaml  --toc --template=style/template.epub3 --epub-stylesheet=style/epub3.css --epub-cover-image=img/DSP6.png --bibliography=wwwrite.bib -o WWWRITE.epub  

As you may notice, the command is a bit more "complex" than the previous HTML one, yet basically resembles it in many ways.

The arguments

	-S index.md Lesson1.md Lesson2.md Lesson3.md Lesson4.md metadata.epub3.yaml -o WWWRITE.epub  

specify a list of _input_ files (index.md, Lesson1.md, Lesson2.md, Lesson3.md, Lesson4.md, and metadata.epub3.yaml) and an _output_ file (-o WWWRITE.epub). Pandoc will concatenate input files into a single one when listed as inputs. This is convenient as it enables a writer to segment a book into self-contained chapters for example. 

Styling and template options are specified with 

	--template=style/template.epub3 --epub-stylesheet=style/epub3.css  --bibliography=wwwrite.bib

these are exactly the same as in the previous HTML example. In fact, in one looks at the template and CSS you could notice that that are to some extent very similar to the HTML5 ones. This is because the EPUB markup language is based on HTML! 

Also notice the 

	--epub-cover-image=img/DSP6.png

which tells pandoc to use the specified image as cover for the EPUB.

Finally the argument

	--toc

is a feature that instructs pandoc to generate a "Table Of Content" (toc) out of the various markdown Headings (symbol '#' [Lesson3](Lesson3.html)) present in the input files.

Let's now have a look at the EPUB output!

	
### Extra

[Try pandoc!](http://johnmacfarlane.net/pandoc/try/)

[Pandoc User's Guide](http://johnmacfarlane.net/pandoc/README.html#epub-metadata)

[Getting Started](http://johnmacfarlane.net/pandoc/getting-started.html)
