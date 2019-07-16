This is a tool for labeling text documents.
Slate supports annotation at different scales (spans of characters, tokens, and lines, or a document) and of different types (free text, labels, and links).
This covers a range of tasks, such as Part-of-Speech tagging, Named Entity Recognition, Text Classification (including Sentiment Analysis), Discourse Structure, and more.

Why use this tool over the range of other text annotation tools out there?

- Fast
- Trivial installation
- Focuses all of the screen space on annotation (good for large fonts)
- Terminal based, so it works in constrained environments (e.g. only allowed ssh access to a machine)
- Not difficult to configure and modify

## Installation

Three options:

### 1. Install with pip
```bash
pip install slate-jkk
```
Run with either of:
```
slate
python -m slate
```

### 2. Download the repository
```bash
curl https://codeload.github.com/jkkummerfeld/slate/zip/master -o "slate.zip"
unzip slate.zip
cd slate-master
```
Run with either of:
```
python slate.py
./slate.py
```
Or from another directory:
```
python PATH_TO_SLATE/slate.py
```

### 3. Clone the repository
```bash
git clone https://github.com/jkkummerfeld/slate
cd slate
```
Run with:
```
python slate.py
```
Or from another directory:
```
python PATH_TO_SLATE/slate.py
```

### Requirements

The code requires only Python (2 or 3) and can be run out of the box.
Your terminal must be at least 80 characters wide and 20 tall to use the tool.

## Citing

If you use this tool in your work, please cite:

@InProceedings{acl19slate,
  title     = {SLATE: A Super-Lightweight Annotation Tool for Experts},
  author    = {Jonathan K. Kummerfeld},
  booktitle = {Proceedings of {ACL} 2019, System Demonstrations},
  location  = {Florence, Italy},
  month     = {July},
  year      = {2019},
  pages     = {},
  url       = {https://github.com/jkkummerfeld/slate/raw/master/paper/acl19slate.pdf},
  software  = {https://jkk.name/slate},
}

## Getting Started

Run `python slate/annotate.py <filename>` to start annotating `<filename>` with labels over spans of tokens.
The entire interface is contained in your terminal, there is no GUI.
With command line arguments you can vary properties such as the type of annotation (labels or links) and scope of annotation (characters, tokens, lines, documents).

The input file should be plain text, organised however you like.
Prepare the data with your favourite sentence splitting and/or tokenisation software (e.g., [SpaCy](https://spacy.io)).
If you use Python 3 then unicode should be supported, but the code has not been tested extensively with non-English text (please share any issues!).

When you start the tool it displays a set of core commands by default.
These are also specified below, along with additional commands.

The tool saves annotations in a separate file (`<filename>.annotations` by default, this can be varied with a file list as described below).
Annotation files are formatted with one line per annotated item.
The item is specified with a tuple of numbers.
For labels, the item is followed by a hyphen and the list of labels.
For links, there are two items on the line before the hyphen.
For example, these are two annotation files, one for labels of token spans and the other for links between lines:

```
==> label.annotations <==
(2, 1) - label:a
((3, 5), (3, 8)) - label:a
(7, 8) - label:s label:a

==> link.annotations <==
13 0 - 
13 7 - 
16 7 - 
```

A few notes:
- The second label annotation is on a span of tokens, going from 5 to 8 on line 3.
- The third label annotation has two labels.
- The line annotations only have one number to specify the item.
- When the same line is linked to multiple other lines, each link is a separate item.

### Tutorials

Included in this repository are a set of interactive tutorials that teach you how to use the tool from within the tool itself.

Task | Command
---- | --------
Labelling spans of text in a document | `python slate.py tutorial/label.md -t categorical -s token -o -hh -l log.tutorial.label.txt`
Linking lines in a document | `python slate.py tutorial/link.md -t link -s line -o -hh -l log.tutorial.link.txt`
Comparing annotations | Coming soon!

### Example Workflow

This tool has already been used for two annotation efforts involving multiple annotators ([Durrett et al., 2017](http://jkk.name/publication/emnlp17forums/) and [Kummerfeld et al., 2018](http://jkk.name/publication/arxiv18disentangle/)).
Our workflow was as follows:

- Create a repository containing (1) the annotation guide, (2) the data to be annotated divided into user-specific folders.
- Each annotator downloaded slate and used it to do their annotations and commit the files to the repository.
- Either the whole group or the project leader went through files that were annotated by multiple people, using the adjudication mode in the tool.

## Detailed Usage Instructions

### Invocation options

```
usage: slate.py [-h] [-d DATA_LIST [DATA_LIST ...]] [-t {categorical,link}]
                   [-s {character,token,line,document}] [-c CONFIG_FILE]
                   [-l LOG_PREFIX] [-ld] [-hh] [-r] [-o] [-ps] [-pf]
                   [--do-not-show-linked] [--alternate-comparisons]
                   [data [data ...]]

A tool for annotating text data.

positional arguments:
  data                  Files to be annotated

optional arguments:
  -h, --help            show this help message and exit
  -d DATA_LIST [DATA_LIST ...], --data-list DATA_LIST [DATA_LIST ...]
                        Files containing lists of files to be annotated
  -t {categorical,link}, --ann-type {categorical,link}
                        The type of annotation being done.
  -s {character,token,line,document}, --ann-scope {character,token,line,document}
                        The scope of annotation being done.
  -c CONFIG_FILE, --config-file CONFIG_FILE
                        A file containing configuration information.
  -l LOG_PREFIX, --log-prefix LOG_PREFIX
                        Prefix for logging files
  -ld, --log-debug      Provide detailed logging.
  -hh, --hide-help      Do not show help on startup.
  -r, --readonly        Do not allow changes or save annotations.
  -o, --overwrite       If they exist already, read abd overwrite output
                        files.
  -ps, --prevent-self-links
                        Prevent an item from being linked to itself.
  -pf, --prevent-forward-links
                        Prevent a link from an item to one after it.
  --do-not-show-linked  Do not have a special color to indicate any linked
                        token.
  --alternate-comparisons
                        Activate alternative way of showing different
                        annotations (one colour per set of markings, rather
                        than counts).
```

You may also define arguments in a file and pass them in as follows:

```bash
python slate.py @arguments.txt
```

### Keybindings

The tool shows files one at a time in plain text. Commands are:

Type                        | Key                                                       | Labelling Affect                 | Linking Affect
--------------------------- | --------------------------------------------------------- | -------------------------------- | ---------------------
Movement                    | <kbd>j</kbd> or <kbd>&larr;</kbd>                         | move to the left                 | move selected item to the left
&nbsp;                      | <kbd>i</kbd> or <kbd>&uarr;</kbd>                         | move up a line                   | move selected item up a line
&nbsp;                      | <kbd>o</kbd> or <kbd>&darr;</kbd>                         | move down a line                 | move selected item down a line
&nbsp;                      | <kbd>;</kbd> or <kbd>&rarr;</kbd>                         | move to the right                | move selected item to the right
&nbsp;                      | <kbd>J</kbd> or [<kbd>Shift</kbd> + <kbd>&larr;</kbd>]        | go to the start of the line      | move linking item to the left
&nbsp;                      | <kbd>I</kbd> or [<kbd>Shift</kbd> + <kbd>&uarr;</kbd>]        | go to first line                 | move linking item up a line
&nbsp;                      | <kbd>O</kbd> or [<kbd>Shift</kbd> + <kbd>&darr;</kbd>]        | go to last line                  | move linking item down a line
&nbsp;                      | <kbd>:</kbd> or [<kbd>Shift</kbd> + <kbd>&rarr;</kbd>]        | go to the end of the line        | move linking item to the right
Edit Span                   | <kbd>m</kbd>                                              | extend left                      | extend selected item left
&nbsp;                      | <kbd>k</kbd>                                              | contract left side               | contract selected item left
&nbsp;                      | <kbd>/</kbd>                                              | extend right                     | extend selected item right
&nbsp;                      | <kbd>l</kbd>                                              | contract right side              | contract selected item right
&nbsp;                      | <kbd>M</kbd>                                              | -                                | extend linking item left
&nbsp;                      | <kbd>K</kbd>                                              | -                                | contract linking item left
&nbsp;                      | <kbd>?</kbd>                                              | -                                | extend linking item right
&nbsp;                      | <kbd>L</kbd>                                              | -                                | contract linking item right
Label Annotation (default)  | <kbd>Space</kbd> then <kbd>a</kbd>                        | [un]mark this item as a          | -
&nbsp;                      | <kbd>Space</kbd> then <kbd>s</kbd>                        | [un]mark this item as s          | -
&nbsp;                      | <kbd>Space</kbd> then <kbd>d</kbd>                        | [un]mark this item as d          | -
&nbsp;                      | <kbd>Space</kbd> then <kbd>v</kbd>                        | [un]mark this item as v          | -
Link Annotation             | <kbd>d</kbd>                                              | -                                | create a link and move right / down
&nbsp;                      | <kbd>D</kbd>                                              | -                                | create a link
Either Annotation mode      | <kbd>u</kbd>                                              | undo annotation on this item     | undo all annotations for the current item

Shared commands:

Type                        | Mode   | Key                                             | Affect               
--------------------------- | ------ | ----------------------------------------------- | ----------------------------
Searching                   | Normal | <kbd>\\</kbd>                                    | enter query editing mode
&nbsp;                      | Query  | <kbd>?</kbd> or <kbd>Enter</kbd>                    | exit query editing mode
&nbsp;                      | Query  | <kbd>!</kbd> or <kbd>Backspace</kbd>                    | delete last character in query
&nbsp;                      | Query  | characters except <kbd>?</kbd> and <kbd>!</kbd> | add character to query
&nbsp;                      | Normal | <kbd>p</kbd>                                    | go to previous match
&nbsp;                      | Normal | <kbd>n</kbd>                                    | go to next match
&nbsp;                      | Normal | <kbd>P</kbd>                                    | go to previous match for linking line
&nbsp;                      | Normal | <kbd>N</kbd>                                    | go to next match for linking line
Assigning text labels       | Normal | <kbd>t</kbd>                                    | enter label editing mode
&nbsp;                      | Label  | <kbd>?</kbd> or <kbd>Enter</kbd>                    | exit label editing mode and assign the label
&nbsp;                      | Label  | <kbd>!</kbd> or <kbd>Backspace</kbd>                    | delete last character in label
&nbsp;                      | Label  | characters except <kbd>?</kbd> and <kbd>!</kbd> | add character to label
Saving, exiting, etc        | Normal | <kbd>]</kbd>                                    | save and go to next file         
&nbsp;                      | Normal | <kbd>[</kbd>                                    | save and go to previous file     
&nbsp;                      | Normal | <kbd>q</kbd>                                    | save and quit                    
&nbsp;                      | Normal | <kbd>s</kbd>                                    | save                             
&nbsp;                      | Normal | <kbd>Q</kbd>                                    | quit                             
Misc                        | Normal | <kbd>#</kbd>                                    | toggle line numbers
&nbsp;                      | Normal | <kbd>h</kbd>                                    | toggle help info (default on)    
&nbsp;                      | Normal | <kbd>{</kbd> or <kbd>PAGE-UP</kbd>              | shift view up 5 lines
&nbsp;                      | Normal | <kbd>}</kbd> or <kbd>PAGE-DOWN</kbd>            | shift view down 5 lines
&nbsp;                      | Normal | <kbd>></kbd> then <kbd>p</kbd>                  | toggle showing progress through files
&nbsp;                      | Normal | <kbd>></kbd> then <kbd>l</kbd>                  | toggle showing legend for labels
&nbsp;                      | Normal | <kbd>></kbd> then <kbd>m</kbd>                  | toggle showing the mark on the current item

Note: special keys such as `ENTER` and `BACKSPACE` may not work on non-OS-X operating systems. That is why in all places where they are used we have an alternative as well.

### Misc

To annotate multiple files, specify more than one as an argument.
For greater control, provide a list of files in a file specified with `--data-list`.
The list should be formatted as follows, where [] indicate optional values:

```
raw_file [annotation_file [starting_position [additional_annotation_files]]]
```

For example, these commands will create a file list, use it, then return to it later:

```bash
find . -name *txt > filenames_todo
./slate.py -d filenames_todo -l do_later
# ... do some work, then quit, go away, come back...
./slate.py -d do_later.todo -l do_even_later
```

When the `additional_annotation_files` are included it activates an adjudication mode.
By default, all annotations that appear in all additional files are added to the current annotations.
Disagreements are coloured in the text, but will disappear once a decision is made (using the normal annotation commands).

Note - adjudication has not been as thoroughly tested.
Please share any issues you come across!

## Customisation

Colours and keys are customisable. For labelling, the default is:

 - Underlined, current selected item
 - Green on black, 'a' items
 - Blue on black, 's' items
 - Magenta on black, 'd' items
 - Red on black, 'v' items
 - Cyan on black, multiple types for a single token

For linking, the default is:

 - Underlined, current selected item
 - Green on black, current linking item
 - Blue on black, item is linked to the current linking item
 - Yellow on black, item is in some link, though not with the current linking item

### Modifying the Code

Slate has a relatively small codebase (~2,200 lines) and is designed to make adding new functionality not too hard.
The code is divided up as follows:

 - `annotate.py`, the main program, this has the core loop that gets user input.
 - `config.py`, contains the default configuration, including colours and keyboard bindings.
 - `data.py`, classes to read, store and write data.
 - `view.py`, rendering the screen.

Logic for determining what colour goes where is split across two parts of the code.
In `data.py`, the set of labels for an item is determined.
In `view.py`, that set of labels is used to choose a suitable colour.

Adding a new command involves:

 - Adding the name and key to `input_action_list` in `config.py`
 - Adding a mapping from the name to a function in `action_to_function` in `annotate.py`
 - Adding or modifying a function in `annotate.py`
 - Modifying `data.py` or `view.py` to apply the action

# Questions

If you have a question please either:

- Open an issue on [github](https://github.com/jkkummerfeld/slate/issues).
- Mail me at [jkummerf@umich.edu](mailto:jkummerf@umich.edu).

# Contributions

If you find a bug in the code, please submit an issue, or even better, a pull request with a fix.

# Acknowledgments

This tool is based in part upon work supported by IBM under contract 4915012629, and by ONR under MURI grant N000140911081.
Any opinions, findings, conclusions or recommendations expressed are those of the authors and do not necessarily reflect the views of IBM.

