# Tagged Bible data

This repository contains generated, compact word-level Strong's data used by the Walk as Jesus Bible Study page.

## Contents

- `kjv/`: CrossWire KJV (1769) Strong's word groups and a rendering concordance.
- `svv/`: CrossWire Dutch Statenvertaling (`DutSVVA`) Strong's word groups and a Dutch rendering concordance.

Every book is a `json.gz` file. `_usage.json.gz` maps a Strong's number to its most common renderings, their counts and references. The data is generated from public CrossWire SWORD modules; it is not edited by hand.

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

The command downloads the configured CrossWire modules, removes study notes and headings, filters morphology codes, and writes the compressed data files. It also writes the local public-domain Statenvertaling text to `data/local_bibles/statenvertaling/`, which remains in the parent repository because it is an application Bible source rather than alignment metadata.

For a locally extracted SWORD module:

```bash
python manage.py build_tagged_strongs_index svv --module-path /path/to/DutSVVA
```

## Licensing and sources

- KJV source: `KJV` module, `DistributionLicense=GPL`.
- Statenvertaling source: `DutSVVA` module, `DistributionLicense=Public Domain`.
- Module index: https://www.crosswire.org/ftpmirror/pub/sword/packages/rawzip/

The parent project is GPL-3.0. Keep source attribution and licensing metadata intact when regenerating or publishing this data.
