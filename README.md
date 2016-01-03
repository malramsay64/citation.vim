citation.vim
============

A citation source for unite.vim

Imports zotero databases (with better_bibtex citation keys if available) or
exported bibtex files.

Use your markdown documents as a document manager - open referenced pdfs
and urls directly from your citations or from a citation list. Browse all
citation details, notes and abstracts within vim. Yank, insert or preview just
about any of them.

Best used with zotero automatically downloading pdfs. Automatically
export bibtex files with zotero_better_bibtex exports or just use the zotero
database directly. 

### Sources

This version of the plugin provides a *lot* of sources.

bibtex/key
- returns the bibtex key string like [@smith2004] to be used as a reference.
- customisable prefix and suffix for latex/pandoc citations

bibtex/file
- Returns the file attached to a citation, great for opening pdfs from vim
  directly, using the start action.
- If there are multiple files it just returns the first one.

bibtex/combined
- Preview all available citation data on one page.

And everything else! Some more useful than others...

bibtex/abstract
bibtex/author
bibtex/date
bibtex/doi
bibtex/isbn
bibtex/issue
bibtex/journal
bibtex/language
bibtex/month
bibtex/note
bibtex/pages
bibtex/publisher
bibtex/tags
bibtex/title
bibtex/type
bibtex/volume

### Examples mappings:

Set a unite leader:
nmap <leader>u [unite]
nnoremap [unite] <nop>

To insert a citation:

    nnoremap <silent>[unite]c       :<C-u>Unite -buffer-name=bibtex   -start-insert -default-action=append      bibtex<cr>

To immediately open a file or url from a citation under the cursor:

    nnoremap <silent><leader>co :<C-u>Unite -input=<C-R><C-W> -default-action=start -force-immediately bibtex/file<cr>

To immediately browse the folder from a citation under the cursor:

    nnoremap <silent><leader>cf :<C-u>Unite -input=<C-R><C-W> -default-action=file -force-immediately bibtex/file<cr>

To view all citation information from a citation under the cursor:

    nnoremap <silent><leader>ci :<C-u>Unite -input=<C-R><C-W> -default-action=preview -force-immediately bibtex/combined<cr>


To preview, append, yank citation data from unite:

    nnoremap <silent>[unite]c  :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/key<cr>
    nnoremap <silent>[unite]cF :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/file<cr>
    nnoremap <silent>[unite]ca :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/author<cr>
    nnoremap <silent>[unite]cp :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/publisher<cr>
    nnoremap <silent>[unite]cj :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/journal<cr>
    nnoremap <silent>[unite]ct :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/title<cr>
    nnoremap <silent>[unite]cn :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/annote<cr>
    nnoremap <silent>[unite]cb :<C-u>Unite -buffer-name=bibtex -default-action=append  -auto-preview bibtex/abstract<cr>
    nnoremap <silent>[unite]cI :<C-u>Unite -buffer-name=bibtex -default-action=preview -auto-preview bibtex/combined<cr>

To preview, append, yank citation data from unite:
    nnoremap <silent>[unite]cc :<C-u>Unite -buffer-name=bibtex -start-insert -default-action=preview              bibtex/combined<cr>



### Requirements

- Vim with python2 or python3 scripting enabled.
- [unite.vim](https://github.com/Shougo/unite.vim)
- [pybtex](http://pypi.python.org/pypi/pybtex): A BibTeX-compatible bibliography processor in Python

### Usage

1. Install [unite.vim](https://github.com/Shougo/unite.vim)
1. Install this plugin
1. If you're using bibtex install [pybtex](http://pypi.python.org/pypi/pybtex)

  `sudo easy_install pybtex`

  - Set variables
      let g:citation_vim_file_path=["/path/to/your/bib/file/library.bib"]
      let g:citation_vim_file_format="bibtex"

1. To use [zotero](http://pypi.python.org/pypi/pybtex)
  - Set variables

      let g:citation_vim_file_path=["/path/to/your/zotero/7XX8XX72/zotero/folder/"]
      let g:citation_vim_file_format="zotero"

1. Set your citation suffix and prefix. Pandoc markdown style is the default.

let g:citation_vim_outer_prefix="["
let g:citation_vim_inner_prefix="@"
let g:citation_vim_suffix="]"


### Tweaks 

Customise the unite display, using the names of bibtex sources and a python format string:

    let g:citation_vim_description_format = "{}∶ {} \˝{}\˝ ₋{}₋ ₍{}₎"
    let g:citation_vim_description_fields = ["type", "key", "title", "author", "year"]

or this one is nice for showing journal/publisher (citations rarely have both):

    let g:citation_vim_description_format="{}→ ′{}′ ₊{}₊ │{}{}│"
    let g:citation_vim_description_fields=["key", "title", "author", "publisher", "journal"]

Highlighting picks up text between some weird characters. Nothing on the keyboard, as they will be in
the citation text too. 

Copy and paste in from this list:
- Apostrophes and quotes  ˝‘’‛“”‟′″‴‵‶‷
- Brackets                ⊂〔₍⁽     ⊃〕₎⁾ 
- Arrows                  ◀◁<‹    ▶▷>› 
- Blobs                   ♯♡◆◇◊○◎●◐◑∗∙⊙⊚⌂★☺☻▪■□▢▣▤▥▦▧▨▩
- Tiny                    、。‸₊⁺∘♢☆☜☞♢☼
- Bars                    ‖│┃┆∥┇┊┋
- Dashes                  ‾⁻−₋‐⋯┄–—―∼┈─▭▬┉━┅₌⁼‗

- And use these like a colon after words (notice that's not a normal colon!)
        ∶∷→⇒≫ 

Tip: Use quoted fields near the middle. 

Long lines will occasionally break the display. Its how unite works and theres
not much we can do about it unfortunately.

### Troubleshooting

You can correct your .bib file with `pybtex-convert`:

    pybtex-convert /path/to/your.bib out.bib
