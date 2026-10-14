# Tagged Bible data

This submodule contains generated, compact word-level Strong's alignment data used by the Walk as Jesus Bible Study page. It is not the readable Bible text shown in translation cards; the bundled Statenvertaling verse text lives separately in `walkasjesus_website/data/local_bibles/statenvertaling/`.

## Contents

- `kjv/`: CrossWire KJV (1769) Strong's word groups and an English rendering concordance.
- `svv/`: CrossWire Dutch Statenvertaling (`DutSVVA`) Strong's word groups and a Dutch rendering concordance.

Every book is a `json.gz` file containing verse-level runs: each run has a Strong's code and the translated words that CrossWire tags with that code. The `_usage.json.gz` index maps Strong's codes to their most common renderings, counts and references.

The Bible Study app combines these files with the Hebrew/Greek words and Strong's candidates from STEPBible and its local lexicons. This enables word-by-word highlighting and occurrence lookups. For Dutch, `svv/` supplies the Statenvertaling wording displayed under each original-language word and the Dutch occurrence counts. The separate local verse-text files supply the actual selectable Statenvertaling translation.

These files are generated from public CrossWire SWORD modules; do not edit them by hand. To regenerate them, follow the instructions below. The command writes tagged files here and, for `svv`, also refreshes the parent project's local verse text at `walkasjesus_website/data/local_bibles/`. See that directory's `README.md` in the parent `walkasjesus_framework` repository for the local text format and its role in the app.

## Get the version used by the website

From the framework root:

```bash
./walkasjesus_website/update_tagged_bibles.sh
```

That initialises this submodule at the exact revision recorded by the parent project and prints that revision.

## Regenerate data

From `walkasjesus_website`:

```bash
pip install pysword
python manage.py build_tagged_strongs_index
```

The command downloads the configured CrossWire modules, removes study notes and headings, filters morphology codes, and writes the compressed tagged data files here. For `svv`, it also writes the matching public-domain Statenvertaling verse text to the parent repository at `data/local_bibles/statenvertaling/`.

For a locally extracted SWORD module:

```bash
python manage.py build_tagged_strongs_index svv --module-path /path/to/DutSVVA
```

## Licensing and sources

- **KJV tagging and text:** CrossWire SWORD [`KJV` module ZIP](https://www.crosswire.org/ftpmirror/pub/sword/packages/rawzip/KJV.zip), module `KJV`, version 3.1, `DistributionLicense=GPL`. Its module metadata credits The Bible Foundation for the OT Strong's data, the KJV2003 Project at CrossWire for NT Strong's data, Dr. Maurice Robinson for the base Greek text, and eBible.org for OSIS markup. The metadata says the KJV2003 contribution is offered for any purpose under CrossWire Bible Society's public license. It also notes Crown copyright restrictions for printing the KJV in the UK; check the module's full `About` metadata and applicable local requirements before redistributing the included KJV wording.
- **Statenvertaling text and tags:** CrossWire SWORD [`DutSVVA` module ZIP](https://www.crosswire.org/ftpmirror/pub/sword/packages/rawzip/DutSVVA.zip), module `DutSVVA`, version 1.0, `DistributionLicense=Public Domain`. Its `About` metadata identifies the historic Dutch Statenvertaling commissioned by the States-General and translated from the original languages following the Synod of Dordrecht (1618-1619).
- **Module catalogue:** [CrossWire raw SWORD packages](https://www.crosswire.org/ftpmirror/pub/sword/packages/rawzip/).
- **Data format and reader:** [pysword](https://github.com/fruiz500/pysword) reads the SWORD modules during generation; its code is not bundled into these generated runtime datasets.

The generated files are adapted from those modules: study notes/headings and non-lexical morphology codes are removed; the Dutch export also includes plain verse text. Preserve the source names, module identifiers, license declarations and attribution above when publishing derivatives. This is source documentation, not legal advice; the KJV module itself calls out the UK Crown copyright caveat.

The parent project is GPL-3.0. Keep source attribution and licensing metadata intact when regenerating or publishing this data.
