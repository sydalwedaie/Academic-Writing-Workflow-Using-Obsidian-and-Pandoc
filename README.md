# Pandoc-Based Setup to Convert Markdown to Properly Formated APA Student Paper

APA student papers need to be formatted in a specific way. I developed this workflow to allow students to write their papers in plain text, and have them automatically converted to a properly formatted paper based on APA guidelines.

## The Problem: Word Processors are a Hassle

- Writing straight in a word processor is distracting. Instead of focusing on thinking and writing, it diverts your mental capacity to fiddling around with endless styling and menus.
- An APA paper follows very specific formatting from text styles to paper layouts, which are all labor intensive to do in a word processor.
- Writing math is basically a nightmare!

## The Goal:

- Write in Markdown. Focus on semantics instead of styles.
- Allow easy copy-pasting to and from a grammar and plagiarism checker, without worrying about formatting issues and in-text assets (images, math formalae, tables, etc.)
- Allow writing Math in LaTex.
- Export to DOCX a properly formatted APA student paper in one step.

## The Solution: Pandoc With Custom Template

Pandoc is a universal document converted. It can convert between pretty much any format in any direction. It uses a default **template file** for the structure of the output (title, headers and footers, body, etc), and a default **styles file** for styling the document (font sizes, heading styles, etc). Both can be overridden with custom files. This repository provides these custom files, and a framework to write a paper completely in Markdown and have Pandoc convert it to a finished paper in DOCX format.

## The Workflow

Install Pandoc.
Download this repository to your desktop.
Use the `input.md` file as a template to write the paper in markdown, as per the framework specified below.
Run the following command, making sure it correctly references the files.

```sh
pandoc input.md --reference-doc=custom-reference.docx --template=template.openxml --lua-filter=filter.lua -o output.docx
```

`output.docx` is the finished paper. Feel free to open it in a word process for additional editing.

## The Framework

The workflow expects an input markdown file. It should have a front matter. Various Markdown syntax is used to defined specific areas of an APA paper.

### YAML Front Matter

The front matter holds information for the cover page and the Abstract. Note the following:

- The order of fields doesn't matter.
- If your paper doesn't need an abstract, remove the “paper-abstract” field entirely (including the key itself). An empty “paper-abstract” field would result in a blank page.
- YAML front matter is space-sensitive. Do not leave empty lines after inside the front matter next to the opening and closing lines (`---`).

### Basic Text

Normal text
Bold
Italic

### Headings

APA heading 1, 2, and 3 correspond respectively to markdown headings level 1, 2, and 3.
APA heading 4 should be placed in a bold syntax, following by a perid and the following paragraph on the same line.
APA heading 5 should be placed in a bold AND italic syntax, following by a perid and the following paragraph on the same line.

Note: Markdown Level 6 heading is reserved for the references.

### Figures

Each figure or table along with their associated information must be wrapped in a fenced block as per this example:

```md
::: { custom-style="Figure" }
**Figure #**

_Figure description in italics_

![](figure.ext)

_Note:_ Figure notes.
:::
```

Note the following

- Observe the blank lines between the 4 components. This is mandatory to have them ouput on seperate lines.
- Do not write an `Alt` text for the image (inside the brackets). Pandoc will output this as a caption below the image, which is not an APA standard.

### References

The references section should follow a horizontal rule, denoted with tripple dashes. The rule will be converted to a _page break_ in the final paper.

```md
---

# References

###### Reference 1

###### Reference 2
```

Note the blank lines between each component.

Start the section with a level 1 heading, and use level 6 heading for each reference. The styles are set in such a way that convert level 6 headings to regular text with hanging indent.
