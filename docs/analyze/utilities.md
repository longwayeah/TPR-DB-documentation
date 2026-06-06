---
icon: lucide/library-big
title: Utilities
description: A description of different utilities (toolkits) that help researchers analyze translation process research data from the CRITT Translation Process Research Database (TPR-DB)
---

# Utilities

## `tprdb-utilities` (Python Library)

The Python library `tprdb-utilities` ([Python Package Index](https://pypi.org/project/tprdb-utilities/) | [GitHub Repo](https://github.com/Critt-Kent/tprdb-utilities)) is a library that makes it easy to fetch data tables from specific TPR-DB studies and then read them into Pandas DataFrames for analysis.

### `fetcher`

The `fetcher` module and its `fetch_TPRDB_tables()` function works with the [TPR-DB web app](https://critt.as.kent.edu/tpr/)'s API to download/update data tables for a specific TPR-DB study, saving the most up-to-date tables in a location of your choosing.

!!! tip "Why fetch?"

    If you need to analyze the data from a location other than the CRITT TPR-DB Jupyter server, this module/function makes it **super easy to download and organize the data tables** 🤓

### `reader`

The `reader` module and its `read_TPRDB_tables()` function takes the data tables you have downloaded for one or more studies you specify and reads them into a Pandas DataFrame object for subsequent analysis.

If working directly from the CRITT TPR-DB Jupyter server, the same module/function will also read the data directly from where the TPR-DB stores it.

## Progression Graphs

Placeholder text

### Shiny R