![Version](https://img.shields.io/github/v/release/Chorale-Corpus/Bach_JS?display_name=tag)
<!-- Zenodo DOI badge: insert after the first archived release, e.g.
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXXX) -->
![GitHub repo size](https://img.shields.io/github/repo-size/Chorale-Corpus/Bach_JS)
![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-9cf)


This is a README file for a data repository originating from the [Chorale Corpus initiative](https://github.com/Chorale-Corpus), a multi-institutional effort to bring together a wide collection of digital chorale transcriptions for musicians and researchers. This repository is a sub-corpus of the meta-corpus [Chorale-Corpus/data](https://github.com/Chorale-Corpus/data) ([DOI 10.5281/zenodo.17229783](https://doi.org/10.5281/zenodo.17229783)).

When you use (parts of) this dataset in your work, please read and cite the accompanying data report:

_Gerhardt, K., Hentschel, J., Phan, V. D., Kirsch, M., Kirsch, K., & Gotham, M. R. H. (2026). Beyond Ba(t)ch: A Multimodal Meta-Corpus of Digital Chorale Transcriptions and Related Data. Transactions of the International Society for Music Information Retrieval, 9(1), 440–455. https://doi.org/10.5334/tismir.226_

# A digital edition of the 389 Chorale Settings (Choralgesänge) by J.S. Bach

This corpus has been made possible by Gertim Alberda, who 'manually' digitalized all 389 Bach four-voice chorales as published by the Breitkopf edition no. 3765 (see the included submodule [`389_chorale_settings`](https://github.com/johentsch/389_chorale_settings), which contains the authoritative MuseScore files in two versions: `original_complete` and `vocal_parts_only`). Only the `vocal_parts_only` version (without instrumental parts) is included in the file conversions of this repository and in the Chorale Corpus at large.

Unique within the Chorale Corpus, this sub-corpus additionally provides note-level [score-to-audio alignments](#score-to-audio-alignments) with a commercial recording for 303 of the chorales.

## Getting the data

* download the repository as a [ZIP file](https://github.com/Chorale-Corpus/Bach_JS/archive/refs/heads/main.zip) — note that GitHub ZIP archives do not include submodule content, so this download comes without the MuseScore source files (the converted formats are included) — see [Download of the MuseScore sources](#download-of-the-musescore-sources)
* download a [Frictionless Datapackage](https://specs.frictionlessdata.io/data-package/) that includes concatenations of the TSV files in the folders `measures`, `notes`, and `chords`, and a JSON descriptor:
  * [bach_js.zip](https://github.com/Chorale-Corpus/Bach_JS/releases/latest/download/bach_js.zip)
  * [bach_js.datapackage.json](https://github.com/Chorale-Corpus/Bach_JS/releases/latest/download/bach_js.datapackage.json)
* clone the repo: `git clone --recursive https://github.com/Chorale-Corpus/Bach_JS.git` (the `--recursive` flag pulls in the included submodule)

## File formats

Information related to one and the same chorale is represented in multiple variants and formats. Each comes with its own advantages and disadvantages and users should make informed choices. Filenames function as IDs in this context in the sense that files representing (information from) the same chorale share the same filename prefix.

### Authoritative file format: MSCX

At the moment, only one file format in this dataset can be trusted to contain the full amount of information to the highest degree of accuracy: the uncompressed MuseScore files ending on `.mscx` which can be opened with MuseScore 3 or 4. Contrary to the other sub-corpora of the Chorale Corpus, these files are not part of this repository itself: they live in the nested submodule [`389_chorale_settings`](https://github.com/johentsch/389_chorale_settings) (folders `original_complete/MS3` and `vocal_parts_only/MS3`), and all conversions and TSV extractions in this repository derive exclusively from the `vocal_parts_only` version. To date, we allow modification of these files using [MuseScore version 3.6.2](https://github.com/musescore/MuseScore/releases/tag/v3.6.2) exclusively. However, we use the latest version of MuseScore 4 (v4.4.4 at the time of writing this in February 2025) to convert these files to MEI and musicXML. Apart from these format, the information from the MuseScore files is accessible by means of tabular files in TSV format, 3 per chorale: `*.notes.tsv`, `*.measures.tsv`, and `*.chords.tsv` (although the naming of the last is misleading as it contains mainly markup, lyrics, bass figures, etc.).

The latest version of the Python library `ms3` is used to batch convert the MuseScore files to other formats (`ms3 convert`) and to extract score information to TSV files (`ms3 extract`).

### MEI

To date, MuseScore 4 is able to convert files to MEI Basic 5.0 format. Take these files with a grain of salt as we cannot guarantee congruence with the source files. The quality of these files makes them unsuitable for music research but they may serve as a starting point for a well-curated scholarly edition. In the long run, provided the maturing of the relevant tools, the MEI files should take on the role of being the authoritative format. Until then, they should not be manually modified because they are to be re-generated by conversion and overwritten once the authoritative MuseScore files are modified.

### musicXML

For convenience and in addition, we offer the chorales in musicXML format. However, experience shows that musicXML files output by MuseScore come with a number of issues and conversion errors. These files are unsuited for scholarly work but some users may still appreciate their availability.

### TSV files

Tab-separated files are a dialect of CSV files and can be used the exact same way. The most convenient way of viewing them is through a spreadsheet program such as LibreOffice Calc (Excel, Numbers, Sheets, etc.) or a text editor with TSV support/plugin. Power users may want to load them in their favourite programming language or statistical software.

You can look up what any column means in the documentation of ms3: https://ms3.readthedocs.io/columns

The most important TSV file is called `metadata.tsv`. It contains one row per chorale, and comes with a number of columns that describe the piece in numerous ways. A synoptic overview of the most important columns can be found [here](https://dcmlab.github.io/mozart_piano_sonatas/#how-to-read-metadata-tsv).

### Loading TSV files in Python

Since the TSV files contain null values, lists, fractions, and numbers that are to be treated as strings, you may want to use this code to load any TSV files related to this repository (provided you're doing it in Python). After a quick `pip install -U ms3` (requires Python 3.10 or later) you'll be able to load any TSV like this:

```python
import ms3

notes = ms3.load_tsv("notes/B001.notes.tsv")
metadata = ms3.load_tsv("metadata.tsv")
```

Each TSV file comes with its own JSON descriptor (`*.resource.json`) that describes the meanings and datatypes of its columns ("fields"), follows the [Frictionless specification](https://specs.frictionlessdata.io/tabular-data-resource/), and can be used to validate and correctly load the described file.


## Score-to-audio alignments

A unique feature of this sub-corpus: 303 of the 389 chorale settings have been aligned, note by note, with the recordings made by the Chamber Choir of Europe under Nicol Matt in 1999 (CDs 122 through 127 of [this release](https://musicbrainz.org/release/5582b212-aea7-4355-8c4c-531ed438e5fc)). The alignments come as one TSV table per chorale in the folder [`389_chorale_settings/vocal_parts_only/note_alignments`](https://github.com/johentsch/389_chorale_settings/tree/main/vocal_parts_only/note_alignments): unfolded ("playthrough") versions of the notes tables, i.e. with repeats and first/second endings expanded, carrying two additional columns `start` and `end` with the timecodes of every note in the corresponding recording. The audio itself is commercial and cannot be included; the column `mb:recording` of `metadata.tsv` identifies the exact recording of each chorale via its MusicBrainz ID.

Load them like any other TSV file of this corpus (see [above](#loading-tsv-files-in-python)):

```python
import ms3

aligned_notes = ms3.load_tsv("389_chorale_settings/vocal_parts_only/note_alignments/B001_unfolded.notes.tsv")
```

The folder [`example_alignment`](./example_alignment) shows how to combine the alignments with audio: [`177.csv`](./example_alignment/177.csv) is the alignment table of chorale 177 with all but a few columns removed — [Sonic Visualiser](https://sonicvisualiser.org/) only makes the first few columns of an imported file available and needs just `midi`, `start`, and `end` — and [`177.sv`](./example_alignment/177.sv) the resulting session. The synchronization procedure is documented in [`ms3_realtime`](https://github.com/johentsch/ms3_realtime/).

The aligned chorales are also mapped to Spotify: the column `spotify` of `metadata.tsv` contains each recording's Spotify track ID (its MusicBrainz ID is in `mb:recording`), and [`musicbrainz_spotify_aligned.tsv`](./example_alignment/musicbrainz_spotify_aligned.tsv), produced with [`align_metadata.ipynb`](./example_alignment/align_metadata.ipynb), maps CD tracks, MusicBrainz recordings, and Spotify tracks (incl. ISRCs and audio features).



## Version history

See the [GitHub releases](https://github.com/Chorale-Corpus/Bach_JS/releases).

## Questions, Suggestions, Corrections, Bug Reports

Please [create an issue](https://github.com/Chorale-Corpus/Bach_JS/issues) and/or feel free to fork and submit pull requests.

## Cite as

> Gerhardt, K., Hentschel, J., Phan, V. D., Kirsch, M., Kirsch, K., & Gotham, M. R. H. (2026). Beyond Ba(t)ch: A Multimodal Meta-Corpus of Digital Chorale Transcriptions and Related Data. Transactions of the International Society for Music Information Retrieval, 9(1), 440–455. https://doi.org/10.5334/tismir.226

## License

Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)).

![cc-by-nc-sa-image](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)

## Download of the MuseScore sources

⚠️ **In ZIP archives of this repository — including the one deposited on Zenodo — the folder `389_chorale_settings` is empty.** It is a git submodule pointing to [https://github.com/johentsch/389_chorale_settings](https://github.com/johentsch/389_chorale_settings), and automatically generated archives do not include submodule content. The authoritative MuseScore sources it contains are archived as a dataset of their own on Zenodo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.11358761.svg)](https://doi.org/10.5281/zenodo.11358761)

Download them from [https://doi.org/10.5281/zenodo.11358761](https://doi.org/10.5281/zenodo.11358761) (this DOI always resolves to the latest version) and extract the archive into the `389_chorale_settings` folder — or simply clone the repository with `git clone --recursive https://github.com/Chorale-Corpus/Bach_JS.git`.

## Overview
|file_name|measures|labels|
|---------|-------:|-----:|
|B001     |      10|     0|
|B002     |      11|     0|
|B003     |       8|     0|
|B004     |      10|     0|
|B005     |      10|     0|
|B006     |      17|     0|
|B007     |      11|     0|
|B008     |       8|     0|
|B009     |      16|     0|
|B010     |      12|     0|
|B011     |      10|     0|
|B012     |      10|     0|
|B013     |      10|     0|
|B014     |      10|     0|
|B015     |      20|     0|
|B016     |      16|     0|
|B017     |      12|     0|
|B018     |      12|     0|
|B019     |      12|     0|
|B020     |      10|     0|
|B021     |       8|     0|
|B022     |      22|     0|
|B023     |      17|     0|
|B024     |      11|     0|
|B025     |      12|     0|
|B026     |      12|     0|
|B027     |      12|     0|
|B028     |      12|     0|
|B029     |      12|     0|
|B030     |      21|     0|
|B031     |      13|     0|
|B032     |      12|     0|
|B033     |      10|     0|
|B034     |       8|     0|
|B035     |      15|     0|
|B036     |      29|     0|
|B037     |       9|     0|
|B038     |      12|     0|
|B039     |      12|     0|
|B040     |      12|     0|
|B041     |      12|     0|
|B042     |      16|     0|
|B043     |      14|     0|
|B044     |      14|     0|
|B045     |      18|     0|
|B046     |       8|     0|
|B047     |      19|     0|
|B048     |      17|     0|
|B049     |      17|     0|
|B050     |      17|     0|
|B051     |      15|     0|
|B052     |      12|     0|
|B053     |       6|     0|
|B054     |      12|     0|
|B055     |      12|     0|
|B056     |      12|     0|
|B057     |      16|     0|
|B058     |       8|     0|
|B059     |      11|     0|
|B060     |      14|     0|
|B061     |       8|     0|
|B062     |      16|     0|
|B063     |      16|     0|
|B064     |      14|     0|
|B065     |       9|     0|
|B066     |       9|     0|
|B067     |      16|     0|
|B068     |       9|     0|
|B069     |       9|     0|
|B070     |      16|     0|
|B071     |      14|     0|
|B072     |      13|     0|
|B073     |      13|     0|
|B074     |      12|     0|
|B075     |      11|     0|
|B076     |      12|     0|
|B077     |      24|     0|
|B078     |      13|     0|
|B079     |       8|     0|
|B080     |      12|     0|
|B081     |      22|     0|
|B082     |      22|     0|
|B083     |      18|     0|
|B084     |      18|     0|
|B085     |      16|     0|
|B086     |      10|     0|
|B087     |      10|     0|
|B088     |      10|     0|
|B089     |      10|     0|
|B090     |      10|     0|
|B091     |      20|     0|
|B092     |      10|     0|
|B093     |      13|     0|
|B094     |      10|     0|
|B095     |      17|     0|
|B096     |      16|     0|
|B097     |      17|     0|
|B098     |      26|     0|
|B099     |      27|     0|
|B100     |      13|     0|
|B101     |      13|     0|
|B102     |      13|     0|
|B103     |      14|     0|
|B104     |      13|     0|
|B105     |      20|     0|
|B106     |       9|     0|
|B107     |      10|     0|
|B108     |      10|     0|
|B109     |      10|     0|
|B110     |      10|     0|
|B111     |      12|     0|
|B112     |      10|     0|
|B113     |      16|     0|
|B114     |       8|     0|
|B115     |      13|     0|
|B116     |      11|     0|
|B117     |      38|     0|
|B118     |      16|     0|
|B119     |      20|     0|
|B120     |      11|     0|
|B121     |      11|     0|
|B122     |      22|     0|
|B123     |      17|     0|
|B124     |      12|     0|
|B125     |      12|     0|
|B126     |      12|     0|
|B127     |      10|     0|
|B128     |      10|     0|
|B129     |      16|     0|
|B130     |      10|     0|
|B131     |      16|     0|
|B132     |      16|     0|
|B133     |      48|     0|
|B134     |      20|     0|
|B135     |      16|     0|
|B136     |      14|     0|
|B137     |       9|     0|
|B138     |      10|     0|
|B139     |       8|     0|
|B140     |      10|     0|
|B141     |      11|     0|
|B142     |      11|     0|
|B143     |      11|     0|
|B144     |      11|     0|
|B145     |       8|     0|
|B146     |       8|     0|
|B147     |      12|     0|
|B148     |      12|     0|
|B149     |      10|     0|
|B150     |      12|     0|
|B151     |      13|     0|
|B152     |      19|     0|
|B153     |      19|     0|
|B154     |      21|     0|
|B155     |      20|     0|
|B156     |      12|     0|
|B157     |      12|     0|
|B158     |      12|     0|
|B159     |      12|     0|
|B160     |      12|     0|
|B161     |      12|     0|
|B162     |      12|     0|
|B163     |      12|     0|
|B164     |      12|     0|
|B165     |      12|     0|
|B166     |      11|     0|
|B167     |      11|     0|
|B168     |      11|     0|
|B169     |      11|     0|
|B170     |       9|     0|
|B171     |      24|     0|
|B172     |      21|     0|
|B173     |      24|     0|
|B174     |      10|     0|
|B175     |      15|     0|
|B176     |      13|     0|
|B177     |      12|     0|
|B178     |      17|     0|
|B179     |      16|     0|
|B180     |      18|     0|
|B181     |      12|     0|
|B182     |       9|     0|
|B183     |      13|     0|
|B184     |      13|     0|
|B185     |      12|     0|
|B186     |      14|     0|
|B187     |      12|     0|
|B188     |      16|     0|
|B189     |      16|     0|
|B190     |      20|     0|
|B191     |      16|     0|
|B192     |      16|     0|
|B193     |      16|     0|
|B194     |      16|     0|
|B195     |      13|     0|
|B196     |      19|     0|
|B197     |      13|     0|
|B198     |      13|     0|
|B199     |      13|     0|
|B200     |      13|     0|
|B201     |      13|     0|
|B202     |      13|     0|
|B203     |      30|     0|
|B204     |      32|     0|
|B205     |      20|     0|
|B206     |      12|     0|
|B207     |      11|     0|
|B208     |       9|     0|
|B209     |       8|     0|
|B210     |      16|     0|
|B211     |      12|     0|
|B212     |      14|     0|
|B213     |      11|     0|
|B214     |      12|     0|
|B215     |      32|     0|
|B216     |      14|     0|
|B217     |      12|     0|
|B218     |       8|     0|
|B219     |      22|     0|
|B220     |      28|     0|
|B221     |      24|     0|
|B222     |      28|     0|
|B223     |      13|     0|
|B224     |      13|     0|
|B225     |      40|     0|
|B226     |      16|     0|
|B227     |      15|     0|
|B228     |      10|     0|
|B229     |      16|     0|
|B230     |      13|     0|
|B231     |      13|     0|
|B232     |      19|     0|
|B233     |      10|     0|
|B234     |      10|     0|
|B235     |      10|     0|
|B236     |      10|     0|
|B237     |       8|     0|
|B238     |       8|     0|
|B239     |       8|     0|
|B240     |      16|     0|
|B241     |       9|     0|
|B242     |      13|     0|
|B243     |      13|     0|
|B244     |      13|     0|
|B245     |      13|     0|
|B246     |      13|     0|
|B247     |      13|     0|
|B248     |      15|     0|
|B249     |      12|     0|
|B250     |      12|     0|
|B251     |      12|     0|
|B252     |      27|     0|
|B253     |       8|     0|
|B254     |      14|     0|
|B255     |      14|     0|
|B256     |      15|     0|
|B257     |      12|     0|
|B258     |      12|     0|
|B259     |      48|     0|
|B260     |       8|     0|
|B261     |      10|     0|
|B262     |      10|     0|
|B263     |      10|     0|
|B264     |       8|     0|
|B265     |       8|     0|
|B266     |       8|     0|
|B267     |      16|     0|
|B268     |      16|     0|
|B269     |      19|     0|
|B270     |      36|     0|
|B271     |      37|     0|
|B272     |      36|     0|
|B273     |      18|     0|
|B274     |       8|     0|
|B275     |      13|     0|
|B276     |      10|     0|
|B277     |      16|     0|
|B278     |      16|     0|
|B279     |      16|     0|
|B280     |      16|     0|
|B281     |      16|     0|
|B282     |      12|     0|
|B283     |      14|     0|
|B284     |      18|     0|
|B285     |      11|     0|
|B286     |      18|     0|
|B287     |      21|     0|
|B288     |       8|     0|
|B289     |      12|     0|
|B290     |      12|     0|
|B291     |      12|     0|
|B292     |      12|     0|
|B293     |      12|     0|
|B294     |      12|     0|
|B295     |      12|     0|
|B296     |      12|     0|
|B297     |      12|     0|
|B298     |      12|     0|
|B299     |      11|     0|
|B300     |      11|     0|
|B301     |      27|     0|
|B302     |      16|     0|
|B303     |      12|     0|
|B304     |      15|     0|
|B305     |      16|     0|
|B306     |      11|     0|
|B307     |      14|     0|
|B308     |      28|     0|
|B309     |      13|     0|
|B310     |      15|     0|
|B311     |      32|     0|
|B312     |      10|     0|
|B313     |      10|     0|
|B314     |      12|     0|
|B315     |      12|     0|
|B316     |      12|     0|
|B317     |      12|     0|
|B318     |      12|     0|
|B319     |      12|     0|
|B320     |      12|     0|
|B321     |      27|     0|
|B322     |      27|     0|
|B323     |       8|     0|
|B324     |      12|     0|
|B325     |      16|     0|
|B326     |      12|     0|
|B327     |      16|     0|
|B328     |      12|     0|
|B329     |      36|     0|
|B330     |      14|     0|
|B331     |      10|     0|
|B332     |      10|     0|
|B333     |      10|     0|
|B334     |      13|     0|
|B335     |      12|     0|
|B336     |      16|     0|
|B337     |      10|     0|
|B338     |      10|     0|
|B339     |      10|     0|
|B340     |      10|     0|
|B341     |      10|     0|
|B342     |      12|     0|
|B343     |      15|     0|
|B344     |      14|     0|
|B345     |      15|     0|
|B346     |      14|     0|
|B347     |      14|     0|
|B348     |      12|     0|
|B349     |      25|     0|
|B350     |      22|     0|
|B351     |      16|     0|
|B352     |      14|     0|
|B353     |      15|     0|
|B354     |      15|     0|
|B355     |      15|     0|
|B356     |      15|     0|
|B357     |      11|     0|
|B358     |       9|     0|
|B359     |       8|     0|
|B360     |      12|     0|
|B361     |      12|     0|
|B362     |      12|     0|
|B363     |      12|     0|
|B364     |      12|     0|
|B365     |      12|     0|
|B366     |      21|     0|
|B367     |       9|     0|
|B368     |       9|     0|
|B369     |      10|     0|
|B370     |       9|     0|
|B371     |       9|     0|
|B372     |       9|     0|
|B373     |       9|     0|
|B374     |      11|     0|
|B375     |      14|     0|
|B376     |      14|     0|
|B377     |      14|     0|
|B378     |      14|     0|
|B379     |      11|     0|
|B380     |      10|     0|
|B381     |      11|     0|
|B382     |      32|     0|
|B383     |      10|     0|
|B384     |      10|     0|
|B385     |      10|     0|
|B386     |      10|     0|
|B387     |      25|     0|
|B388     |      10|     0|
|B389     |       8|     0|


*Overview table automatically updated using [ms3](https://ms3.readthedocs.io/).*
