---
icon: lucide/folder-pen
title: File Naming
description: How to name logging files properly before uploading them to the Translation Process Research Database (TPR-DB).
---

# File Naming

Each file you prepare for the TPR-DB should represent one [session](studies-sessions.md#sessions). The TPR-DB backend expects the following naming convention for all log files (Translog-II or Trados Qualitivity) as well as any accompanying files (InputLog files and eye-tracking files).

## Naming Convention

!!! info "Session Files Naming Convention"

    `P<participant-number>_<modality-abbreviation><source-text-number>.<extension>`

!!! example inline "Ex."

    `P01_T1.xml`

!!! example inline "Ex."

    `P09_P22.xml`

!!! example inline "Ex."

    `P03_MT35.xml`

!!! example inline "Ex."

    `P12_I4.xml`

!!! warning "**Participant** Numbers vs. **Source Text** Numbers"

    Note that single-digit participant numbers are expected to start with **0**

    (e.g., participant 1 = **01**).

    ---

    On the other hand, single-digit source text numbers do **not** start with **0**

    (e.g., source text 1 = 1)

## Standard Modality Abbreviations

The following are standardized modality abbreviations that the TPR-DB will automatically recognize:

<div class="dl-inline" markdown="1">

**`T`**

:   from-scratch Translation

**`P`**

:   Post-Editing of Machine Translation

**`E`**

:   Monolingual Editing

**`C`**

:   Text Copying

**`R`**

:   Translation Revision

**`D`**

:   Dictation

**`S`**

:   Sight Translation

**`MT`**

:   Machine Translation output

**`I`**

:   Simultaneous Interpreting

**`ST`**

:   Simultaneous Interpreting with Source Text

**`A`**

:   Authoring

**`L`**

:   Reading

**`LV`**

:   Reading Aloud

**`U`**

:   Monolingual Summarization

**`H`**

:   Monolingual Paraphrasing

</div>

## Custom Modalities

If your experiment involves sessions that the [standardized modality abbreviations](file-naming.md#standard-modality-abbreviations) do not adequately describe, then you can use, label, and describe custom modalities when you upload your session files.

!!! warning "Custom Modality Abbreviations"

    Custom modality abbreviations must be roman alphabetical characters (A–Z) and can be no longer than two characters

Custom modalities can be very useful for organizing different experimental conditions.

!!! example "Use Case"

    Let's say that, in some of your experiment's sessions, the participants are post-editing with the help of quality estimation. You could name those sessions using a modality abbreviation **`PQ`**.
    
    When you upload to the TPR-DB, you will be prompted to add a label and a description for this custom modality.