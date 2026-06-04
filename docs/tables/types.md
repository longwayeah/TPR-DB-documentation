---
icon: lucide/grid-2x2-check
---

# Data Tables (3.0)

The [TPR feature extraction](https://github.com/Critt-Kent/TPR-DB-web-app/tree/main/libs/tables) generates 14 summary tables, each of which describes the relation between the translation process and the translation product from a different angle. Each row in a TPR-DB table describes a process or product unit, while the columns contain the values of the features (or attributes) for the unit. The attributes can be categorical or numerical (integers or real valued numbers).
Eleven of these tables are already part of the previous [TPR-DB version 2.0](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view), and described there in some detail, albeit several features have changed. The [documentation of the CRITT TPR-DB 2.0, (pp. 18 ff)](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view) classifies the tables into:


| # | Category | Units | Abbreviations |
|---|----------|-------|---------------|
| 1 | Basic product units | Source token, Target token | ST, TT |
| 2 | Composed product units | Segment summary, Session summary, Alignment groups | SG, SS, AG[^AG] |
| 3 | Basic process units | Keystroke data, Fixation data | KD, FD |
| 4 | Composed process units | Activity unit, Fixation unit, Keystroke unit, Production unit, HORF states, HORF cycles | AU, FU, KU, PU, HS, HC |
| 5 | Usage of external resources (inputlog) | External resources | EX |


[^AG]: in the original version we used the term 'alignment unit' (AU) for this table, which we then changed into alignment group (AG). The short form AU was then used for 'Activity Unit'.

## New Tables in TPR-DB 3.0
With the new Python implementation, the [TPR Feature Extraction](https://github.com/Critt-Kent/TPR-DB-web-app/tree/main/libs/tables) generates three additional process unit tables (Category 4): 

* KU: keystroke units, indicative of automated typing routines  
* HS: [HORF](https://www.sciencedirect.com/science/article/pii/S221503902400002X?via%3Dihub) states, states of hesitation, need for orientation, and fluent typing, indicative of affective-cognitive dynamics
* HC: HORF[HORF States](https://www.sciencedirect.com/science/article/pii/S221503902400002X?via%3Dihub) cycles, coordination of HORF states, indicative of affective-cognitive attumement 

Together with the AU and PU tables the three new processing units form a temporally nested hierarchy of behavioral units, each operating on a different timescale, which jointly span a [Behavioural Translation Style Space](https://arxiv.org/abs/2507.12208)[^newTables]:

[^newTables]: new tables are in bold

| Processing Unit          | Timescale    | Primary Function                                                                           |
| :----------------------- | :----------- | :------------------------------------------------------------------------------------------ |
| Activity Units (AU)      | ~500 ms–2 s  | Sensorimotor integration, characterized by properties of eye–hand coordination patterns    |
| **Keystroke Units (KU)** | **~1–2 s**   | **Automatized typing routines, delimited by short inter-keystroke intervals ([KUI](https://arxiv.org/pdf/2604.01410))**                        |
| Production Units (PU)    | ~3–5 s       | Reflective processing segments, delimited by extended inter-keystroke intervals ([PUBs](https://arxiv.org/pdf/2604.01410)) and local planning activity |
| **[HORF States](https://www.sciencedirect.com/science/article/pii/S221503902400002X?via%3Dihub) (HS)**     | **~10–30 s** | **Perception–action states, states of hesitation, orientation, revision, and production flow**                      |
| **HORF Cycles (HC)**     | **~1–3 min** | **Epistemic–pragmatic sequences, characterized by recurring transitions between HORF states**                   |

The definition of the pause thresholds that define AUs, KUs, FUs, PUs has been adopted to translator-specific processing speed ([see](https://arxiv.org/pdf/2604.01410)) so that the units form a nested, non-overlapping hierarchy. This hierarchy of embedded temporal units captures and structures the organization, control, and monitoring of [affective, behavioral, and cognitive](https://www.degruyterbrill.com/document/doi/10.1515/dsll-2025-0002/html) processes during translation.

Each table has a session-specific features, as well as product and process features, many of which are slightly differentin the TPR-DB 3.0, as discussed next.

