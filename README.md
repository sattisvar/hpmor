# Harry Potter et les Méthodes de la Rationalité

Traduction française de [Harry Potter and the Methods Of Rationality](http://www.hpmor.com) en [version LaTeX](https://github.com/rjl20/hpmor), très très largement basée sur [le travail de AdrienH](https://www.fanfiction.net/s/6910226/1/Harry-Potter-et-les-M%C3%A9thodes-de-la-Rationalit%C3%A9). Travail en cours.

<p align="center">
  <img src="https://raw.githubusercontent.com/yeKcim/hpmor/master/preview.jpg">
</p>


## Ce que j’ai déjà fait
* Extraction du [epub vf](https://www.fanfiction.net/s/6910226/1/Harry-Potter-et-les-M%C3%A9thodes-de-la-Rationalit%C3%A9) (un epub est un zip)
* Dans OEBPS/Text, conversion des fichiers xhtml en tex, via
```sh
for f in *.xhtml; do
 pandoc $f -o ${f%.*}.tex;
done
```
* Quelques substitutions via sed pour LaTeX (« \ldots » en « … » par exemple) et pour coller à la traduction française de Harry Potter (Draco en Drago) :
```sh
for f in *.tex; do {
 sed -z 's/\n\n/⏎/g ; s/\n/ /g ; s/⏎/\r\r/g ; s/Draco/Drago/g s/\\ldots{}/…/g ; s/\\ldots/…/g' $f > ${f%.*}_b.tex
}; done
```
* Automatisation de plusieurs fautes récurrentes :
```sh
for f in *.tex; do {sed -i -e "s/c'est à dire/c'est-à-dire/g" $f}; done
for f in *.tex; do {sed -i -e "s/si il/s'il/g" $f}; done
for f in *.tex; do {sed -i -e "s/quelques un/quelques-un/g" $f}; done
for f in *.tex; do {sed -i -e "s/Serdaigles/Serdaigle/g" $f}; done
for f in *.tex; do {sed -i -e "s/Gryffondors/Gryffondor/g" $f}; done
for f in *.tex; do {sed -i -e "s/Serpentards/Serpentard/g" $f}; done
for f in *.tex; do {sed -i -e "s/Poufsouffles/Poufsouffle/g" $f}; done
…
```
* Dans vim (pas réussi avec sed), remplacement par \later :
```vi
:args S*.tex
:%s/\n\~\n\n\\begin{center}\\rule{0.5\\linewidth}{0\.5pt}\\end{center}\n\n\~/\r\\later/g | update
```
* Réintégration du contenu de ces fichiers tex "autocorrigés" dans les fichiers latex VO (+quelques modifications)
* Balai ne prend pas de s au singulier. Semi-automatisation de la suppression
```sh
for f in *.tex; do {sed -i -e "s/n balais/n balai/g" $f}; done
…
```
* Vérifier que la compilation LaTeX est OK 
* Sortir une première version ([22.02](https://github.com/yeKcim/hpmor/releases/tag/v22.02) !)

## Travail en cours

* Mise en forme des dialogues avec « » et — (14/122)
* Vérification de l’égalité des entrants/sortants {} ou <<~ et ~>>
```sh
for f in *.tex; do {
 open=$(grep -o « $f | wc -l);
 close=$(grep -o » $f | wc -l);
 if test $open != $close; then printf "$f $open $close\n"; fi;
} done
```
* Vérification avec la même technique qu’il y a le même nombre de \\shout, \\scream, \\parsel,… que dans la VO
* Modifications difficiles à automatiser : robes, - au lieu de —, manque des - dans les nombres,…
* Un passage de grammalecte
* Trouver une solution pour les notes de traducteur
* Check erreurs/warnings LaTeX (missing characters,…)
* Correction du script d’export epub (pas de texte traduit dans le script, tous les \\ doivent disparaître, étrange \&nbssp;, tableau pas esthétique,…)
* etc.

## Restera à faire

* Relecture linéaire complète pour retrouver les traductions bancales et fautes non corrigées automatiquement.
* Attendre des contributions 😁


# Harry Potter and the Methods Of Rationality

A LaTeX version of [the popular didactic fan-fiction](http://www.hpmor.com)
by Eliezer Yudkowsky, which can make a PDF e-book (one file) or printable
books (either one or six volumes; the latter option is more practical to
bind). There are also dust jackets for the printable volumes.

TeXLive 2015 or later and git are required to build the book. (Note: the
book must be built from a git checkout.)

Note: the Omake Files chapters (11 and 64) have been moved to the end of the
single-file PDF. Those chapter numbers are omitted in the text, so chapter
10 is followed by chapter 12, for example. Similarly, the chapter
disclaimers and epigraphs are removed to an appendix. In the six-volume
PDFs, all chapters are renumbered to start from 1 at the start of a book,
and there are no appendices.


## Files

* `hpmor.tex` - the main file
* `hp-format.tex` - mostly sets up memoir
* `hp-markup.tex` - logical markup commands used in the text
* `chapters/` - one file per chapter, included from `hpmor.tex` and the
  individual volumes `hpmor-N.tex`.
* `spelling-list.txt` - a list of words used to spell-check the book.
* `fonts/` - various fonts used
* `latexmkrc` - configures latexmk to run LaTeX to build the PDFs.
* `GNUMakefile` - contains targets to make a Zip of the PDFs and release
  them to GitHub. (Mostly of interest to project maintainers.) `make all`
  does the same as `latexmk` (see below), which may be useful for editor
  integration (e.g. Emacs).


## Building the book(s)

* `latexmk`: Build all PDFs. (If in doubt, just run this command and do
  something else for twenty minutes!)
* `make all`: Build a Zip of the PDFs.
* `latexmk hpmor`: Build the one-volume PDF `hpmor.pdf`
* `latexmk hpmor-N`: Build one of the six individual volumes
  `hpmor-1.pdf` to `hpmor-6.pdf`.
* `latexmk hpmor-dust-jacket-N`: produce the dust jacket for Volume N,
  `hpmor-dust-jacket-N.pdf`. Note that this requires the corresponding
  volume, `hpmor-N.pdf`, to have been built first.
* `latexmk -c`: Remove files produced by building (except PDFs).
* `latexmk -C`: Remove files produced by building (including PDFs).

By default, the dust jackets assume 80gsm plain paper (this affects the
thickness of the book and hence the size of the dust jacket). This can be
configured in `hp-paper-type.tex`; see `papers.tex` for a list of papers.

The exact sizes of dust jackets may vary; the current parameters were taken
from a commercial printer. They can be adjusted in `hp-dust-jacket.tex` as
desired.

Note that the back dust-flap is left for you to add your own text; edit
`hp-dust-jacket.tex` and search for “PUT YOUR BACK DUST-FLAP TEXT HERE!”.
Make sure you remove the percent sign `%` at the start of the line, or your
text will not be printed. (This is a safety feature to make sure that if you
don’t change the text, the placeholder will not appear; instead, you’ll just
get a blank back flap.)

When producing a book with a dust jacket, you may well not want the front
cover as well. To suppress the front cover, use the following incantation:

`latexmk -norc -e '$options="nocover"' -r latexmkrc -g hpmor-1`

Of course, you can replace `hpmor-1` with any other volume, or leave it
out to generate all PDFs with no cover.

To build a single chapter, from the `chapters` directory use the command:

`latexmk -norc -e '$chapter=N' -r ../latexmkrc -g hpmor-chapter-NNN`

Similarly, to build a single appendix or other non-chapter section, from the
top directory use the command:

`latexmk -norc -e '$chapterfile=FILENAME' -r latexmkrc -g FILENAME`


## Contributing

Contributions are most welcome. These fall into three main categories:

1. Textual corrections (where the text differs from the online original
   unintentionally).
2. Textual improvements: fixing straight-up errors in the English (or
   deeper, the sense, story etc.), or “Britfixing”, i.e. replacing
   non-British usages.
3. Design and typography. Improvements to both the PDF and print versions of
   the books are encouraged. See the GitHub bug-tracker for known issues;
   also, search the sources for “FIXME”.
4. Translations. Translations are of course most welcome! A list of known
   translations and one or two hints are given below in the
   [next section](#Translations).

For textual changes other than simple typo or language fixes, please
familiarise yourself with the style guide (below).

The preferred way to submit any improvement is as a GitHub pull request.
Textual corrections can also be submitted as issues in the issue tracker, or
by email to the maintainer.

For the GitHub URL, and email address of the maintainer, see above.


## Style guide

### Spelling

When spell-checking, use `spelling-list.txt` instead of your personal
dictionary, so the results are less dependent on your setup. (The system
dictionary can still of course vary from one setup to another.)

Words that are standard English or part of the Harry Potter universe, or are
otherwise of “global” relevance should be added to `spelling-list.txt`.
Exclamations (“Eeeehhhh”) and other one-offs should be added to the per-file
word lists. (There’s obviously something of a grey area in the middle, e.g.
one-off references to various real and fictional people.)

Emacs users benefit from a `.dir-locals.el` that automatically sets up `spelling-list.txt` as the personal dictionary for all HPMOR files.


### Chapter headings

Chapters that aren’t part of a continuing series look like this:

`\chapter{The Fundamental Attribution Error}`

Chapters that are part of a continuing series look like one of these:

`\partchapter{Working in Groups}{I}`

`\namedpartchapter{Self-Actualization}{SA}{VIII}{The Sacred and the Mundane}`

The first is pretty simple; it’s just the title of the chapter followed by
which part it is.

The second looks like the title of the chapter, then the abbreviation for
the title of the chapter, then the part, then the title of the part.


### First sentences

Normally, a chapter starts like this:

`\lettrine{P}{adma} Patil had finished`

That creates the large initial letter.

If the first paragraph of the chapter is all italics, though, it looks like
this:

    \begin{em}\lettrine{T}{he} red jet of fire took Hannah full in the
    [...]
    blazing green spirals brought down their foe’s Shield Charm.\par\end{em}


### Sections

`\section{Final Aftermath:}`


### Disclaimers and Epigraphs

These have been removed to an appendix, `hp-epigraphs.tex`.


### Miscellaneous

There are some other things relating to newspaper headlines and such; check
the chapters they appear in for the appropriate markup.


### Markup

These are macros defined in `hp-markup.tex`. You should glance through that
file to see what commands are available, and use them instead of direct
markup; for example `\shout` rather than `\textsc`.


## Translations

To translate the book, it is recommended to fork this repository, and check
back from time to time for updates. Also, do open an issue or PR against
this file to add the translation!

It is recommended to use `polyglossia` (not `babel`).


### Known translations

Note: there are other translations of HPMOR; here are listed only
translations of this edition.

* [French](https://github.com/yeKcim/hpmor) (in progress)


<!--  LocalWords:  hpmor tex hp txt latexmkrc latexmk GNUMakefile 80gsm '
 -->
<!--  LocalWords:  norc nocover N' NNN chapterfile FILENAME' Britfixing dir
 -->
<!--  LocalWords:  Eeeehhhh el partchapter namedpartchapter lettrine adma
 -->
<!--  LocalWords:  textsc
 -->
