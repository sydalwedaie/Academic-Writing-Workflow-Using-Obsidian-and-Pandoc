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

- Install [Pandoc](https://pandoc.org/installing.html).
- Download this repository to your desktop.
- Use the `input.md` file as a template to write the paper in markdown, as per the framework specified below.
- Run the following command from the directory of the repository.

```sh
pandoc input.md --reference-doc=custom-reference.docx --template=template.openxml --lua-filter=pagebreak.lua --lua-filter=crossref.lua -o output.docx
```

`output.docx` is the finished paper. Feel free to open it in a word processor for additional editing.

## Markdown Syntax

The following is the basic syntax you will use to write an APA paper. Refer to [this guide](https://www.markdownguide.org/basic-syntax/) for details.

### Basic Text

| Syntax                          | Rendered Output               | Notes                                                                                                         |
| ------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------- |
| This is regular text            | This is regular text          | Just write normally! <br>Put a blank line between paragraphs.<br>Avoid tabs or spaces in front of paragraphs. |
| This is `**bold**` text         | This is **bold** text         | Wrap in double stars.                                                                                         |
| This is `*italic*` text         | This is _italic_ text         | Wrap in single stars.<br>A single underscore (`_`) also works.                                                |
| This is `***bold and italic***` | This is **_bold and italic_** | Wrap in triple stars.<br>A double star and single underscore (`**_`) also works.                              |

### Headings

Start a heading with 1 to 6 hash (`#`) characters, corresponding to headings level 1 to 6 .
Put blank lines above and below each heading.

```md
# heading level 1

## heading level 2

### heading level 3
```

### Block Quotes

Start the paragraph with a `>` and a space. Put blank lines above a below.

```md
> This is a block quote
```

### Lists

Start each item with a dash (`-`) and a space. Repeat for each item.

```md
- item 1
- item 2
- item 3
```

For ordered lists, use numbers and a period

```md
1. item 1
2. item 2
3. item 3
```

**Note:** APA discourages the use of lists in student papers. You will want to write in paragraph format most of the times.

**Note:** If a list item starts with a number and a period, put a backslash (`\`) before the period to _escape_ it (otherwise it will be interpretted as a list number in an ordered list).

```md
- 2020\. What a year!
- 2021\. Recovery starts.
```

You can do this anytime you want to literally display a character that is used for a markdown syntax. So `\*` would output as `*`.

## The Framework

The workflow expects an input markdown file with a front matter. The markdown syntax above is used, with each some syntax mapped to specific areas of an APA paper.

### YAML Front Matter

The front matter holds information for the cover page and the Abstract.

Note the following:

- The order of fields doesn't matter.
- The title provided here will be automatically inserted in the cover page as well as on top of the first body page.
- If your paper doesn't need an abstract, remove the “paper-abstract” field entirely (including the key itself). An empty “paper-abstract” field would result in a blank page.
- YAML is sensitive to white space. Do not leave blank lines inside the front matter.
- Wrap all values in double quotes to avoid potential issues.

### Headings

APA headings 1, 2, and 3 correspond respectively to markdown headings level 1, 2, and 3.

```md
# APA Heading 1

## APA Heading 2

### APA Heading 3
```

APA headings level 4 and 5 are different, as they are essentially the first line of a paragraph and are following by a period. They are both written as bold text, with heading level 5 **also** in italic.

```md
**APA Heading 4.** Paragraph starts on the same line

**_APA Heading 5_** Paragraph starts on the same line
```

**Note:** There is no APA heading level 6. This is convenient, as markdown heading level 6 is reserved for the references in this framework.

### Figures

Each figure (or table, diagram, etc.) along with the associated information must be wrapped in a fenced block as per this example:

```md
::: { custom-style="Figure" }
**Figure #**

_Figure description in italics_

![](figure.ext)

_Note:_ Figure notes.
:::
```

Note the following

- A fenced block is created by wrapping the content in triple colons (`:::`).
- A custom style is set to make the figure block flush left, otherwise it will output is normal text (indented).
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

## Limitations

I avoided using `citeproc` for automatically generating a bibliography based on in-text citations. While sweet, I found it too overkill for simple student papers. You will have to import your references in a reference manager and link it to where you write your paper. It means you will have to keep maintaining yet another application, something that my workflow tried to avoid. What this workflow does is to properly display the references with hanging indents, which is tricky to setup in a word processor.

`pandoc-crossref` is a nice plugin which allows automatic numbering of figures and cross-referencing them. Basically, you will label your figures, and refer to them in your text using those labels. When you export, they will be automatically numbered and referenced. Unfortunately, I could not make it work as per APA guidelines.
