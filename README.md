# Ifoss law book

This repository contains the sources in various formats of the second edition of the IFOSS Law Book.

Any pull request will be authorized only by the copyright holders of the individual contributions.

> **NOTE**: Dont' work on Master. Create a bugfix

```bash
        git checkout -b bugfix
        git branch --set-upstream-to=origin/bugfix bugfix
```
> and work from there

## Some useful commands to generate Markdown:

**Requires ASCIIDOC**

To generate an intermediate docbook document for all .txt (asciidoc) documents in the directory.

    asciidoc -b docbook *.txt

**Requires Pandoc** (any version).


Assuming you have all the docbook documents in the same place (directory)

    find *.xml -exec pandoc -Ss -f docbook -t markdown_strict+footnotes {} -o {}.xml \;

That creates a number of Markdown documents for all docbook documents in the directory

To generate a single HTML file from the Markdown documents, you need to change the internal footnotes references (as you cannot have two identical footnote reference in the same file) I use this simple substitution script, to be add to a subdirectory, which adds a random number to the footnote references across all files and writes them in the subdirectory:

```bash
    #/bin/bash!

    for f in ../*.xml.md

    do

     sed "s/\(\[\^\)\([0-9]\|[0-9][0-9]\|[0-9][0-9][0-9]\)\(\]\)/\1_$RANDOM\_\2\3/g" $f > $(basename $f)

     echo "done with $(basename $f)"

    done
```
**NOTE**: check that the script went well and copy the resulting documents to the main directory.


Some documents have internal references. There is only a handful of them. You can count how many with:

    grep -c "<<sec" *.txt

So instead of scripting it, you will be content with changing them by hand. You only need to seek for something like

    [section\_title](#sec_finland_moral_rights)

and to do two things:

a) Change the referenced text (the text between square brackets) so to have it look like:  
  `  [Moral Rights](#sec_finland_moral_rights) and`
b) Ad the exact part between round brackets next to the relevant section title (where the reference points to) so it looks like:  
`#### Moral Rights {#sec_finland_moral_rights}`


Now you are ready to produce a full HTML or PDF with

```bash
    pandoc -Ss -f /
    markdown_strict+footnotes+auto_identifiers+implicit_header_references+header_attributes /
    -o file.html file.pdf *.xml.md
```

Thank you

Carlo Piana
