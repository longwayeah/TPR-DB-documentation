---
icon: lucide/clock-8
---

# Process Features
Process features capture the affective, cognitive and behavioral dynamics of action (e.g., translation) as they unfold in time. In the context of the TPR-DB 3.0, they comprise keystroke logging measures — such as production pauses and typing bursts — and gaze metrics derived from eye-tracking, including fixation durations, regressive eye movements, and re-reading behavior. 
Many of those features are described in the [TPR-DB version 2.0](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view). 

The definitions of many TPR-DB features, such as `Dur`, `Ins`, `Del`, `FixS`, `FixT`, etc. have not changed in the TPR-DB 3.0, although - due to a redefinition of [word boundaries](#Word Boundaries) and the reimplementation of the [The TPR Pipeline](#The TPR Pipeline) in Python along with several algorithmic changes - their values may be silightly different. In addition, some features are now differently defined. 

This section therefore, mainly focuses on new features or features for which the definition has changed.


## Source and Target Groups and IDs 

The features `STid`, `TTid`, `SGid`, and `TGid` appear in several TPR-DB tables. `STid`and `TTid` are indexes of individual tokens in the source and target text respectively, and are, therefore, integers. `SGid` and `TGid` (may) refer to gropus of words and are, therefore, strings, i.e., the concatenation of several token IDs, e.g., "$\mathtt{8+10}$". 

During [keystroke-to-word mapping](#Keystroke-to-word Mapping) each keystroke is associated with a unique target word. The Id of this target word is the `TTid`, an integer, in the KD file. During [Bilingual Alignment](#Bilingual Alignment) source words and target words are connected in a way such that one target word can be associated with several source words,  i.e., the `SGid`. Each `SGid` is, in turn, aligned with one or more target words, which together are the `TGid`. Thus, the `TTid` is part of the `TGid`, but the latter may contain several elements. The `STid` is smallest number in the source group. 

For instance, a target token "$\mathtt{8}$" can be aligned with source tokens "$\mathtt{8}$" and "$\mathtt{9}$", thus the `SGid` value is "$\mathtt{8+9}$" and `STid` is "$\mathtt{8}$". If this source group is aligned to a group of target words, say, target words "$\mathtt{8}$" and $\mathtt{10}$", the `TGid` has the vaue "$\mathtt{8+10}$".  

A similar mechanism applies to fixations. While keystrokes can only occur in the target text, fixations are observed on source and trarget words. Thus, a fixation may occur on a source word $7$ which is associated with target words "$\mathtt{5}$" and "$\mathtt{6}$". If this target group is associated with source words "$\mathtt{7}$" and "$\mathtt{8}"$, the `SGid` of this fixation is "$\mathtt{7+8}$", while `TTid` is $"\mathtt{5}"$. 

This same principle applies to these features also in the other tables, including AU, KU, PU, ST, TT, etc. where the ID refers to the token, while the group feature relates to the alignment group to which the unit refers.


## Keystroke Units (KUs) and Production Units (PUs)

As for the [TPR-DB version 2.0](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view), also the TPR-DB 3.0 fragments the flow of keystrokes into processing units. Keystroke-based processing units are separated by a pre-defined lag of time between successive keystrokes (aka inter keystroke intervals, IKIs). The TPD-DB 3.0 distinguishes between two tresholds which, respecively, separate Keystroke Units (KUs) and Production Units (PUs). KUs consist of at least one keystroke separated from the next KU by an IKI $\ge$ `KUI` (the KU Interruption threshold); PUs are separated by an IKI $\ge$ `PUB`, a PU Break. Bandaru and others [^Bandaru] define these thresholds as: 

- `KUI` : $2 \times median(\text{within word IKI})$
- `PUB` : IKI duration of the keystroke quantile: 
  $1 - \frac{3*\text{text length in characters}}{\text{number of words}}$ 

KUs and PUs are enumerated in two separate tables. 
KU tables enumerate the sequence of KUs and the intervening keystroke pauses (`KUI`, and `PUB`) in their sequential order. The `Type` of a KU can be one of $\mathtt{I}$, $\mathtt{C}$ or $\mathtt{D}$, depending on whether the keystrokes are only insertions, deletions or both insertions and deletions, respecively. A keystroke pause can be of `Type`  $\mathtt{K}$, (`KUI`) or a $\mathtt{P}$ (`PUB`). 

As for each session, `KUI` $\le$ `PUB`, every PU consists of one or more KU(s). Thus, there is no overlapping between KUs and PUs. 


[^Bandaru]:
    - Bandaru et al. (2026): https://arxiv.org/abs/2604.01410
    - Muñoz and Apfelthaler (2014): https://aclanthology.org/2014.amta-wptp.6.pdf


## Pause Metrics

Lacruz and colleagues[^lacruz] introduce several metrics to compute the relation between text production (i.e., sequences of fluent typing) and pausing, assuming that keystroke pauses are "good indicators of cognitive demand in monolingual language production and in translation." 
Their metrics include, among others: 

- Pause Ratio: $PR = \frac{\text{total pause time in segment}}{\text{total time in segment}}$
    
- Average Pause Ratio: $APR =\frac{\text{average time per pause}}{\text{average time per words}}$
    
- Pause to Word Ratio: $PWR =\frac{\text{number of pauses in segment}}{\text{number of words in segment}}$

The TPR-DB provides basic features for computing these and other pause metrics on the segment level (SG). 
The pause metrics rely on a notion of $\mathtt{pause}$, which has been a topic of discussion and controversy for many years. 
In line with the literature, the [TPR-DB version 2.0](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view) presumed $\mathtt{pause}$ thresholds of $500ms$, $1000ms$, $2000ms$ and $5000ms$. 

Since pausing behavior in the TPR-DB 3.0 is taken to be translator-specific, the $\mathtt{pause}$ thresholds are now also based on the translator-specific values of `KUI` and `PUB` and include $1000ms$ `KUI`, `PUB`, $2 \times$ `PUB` and $4 \times$ `PUB`. 

The following features on the SG level are involved in the TPR-DB Pause Metrics:

- `Dur` : production duration for a segment; duration between the first and the last keystroke.
- `Nedit`: number of times the segment was edited.
- `PreGap` : segment initial keystroke pause; lag between the last keystroke of the previous segment (or beginning of session) and the first keystoke of the current segment.
- `PostGap` : segment final keystroke pause; lag between the last keystroke of the current segment and the first keystoke of the next segment (or end of session).
- `TB`$\mathtt{pause}$: number of typing bursts given the $\mathtt{pause}$ threshold.
- `TG`$\mathtt{pause}$: total duration of pausing (gap) time given the $\mathtt{pause}$ threshold.
- `TD`<pause> : total duration of drafting time given the $\mathtt{pause}$ threshold.

Note that  `Dur` = `TB` + `TG`. That is, neither the typing pause preceeding the typing events in a segment not the pause following it are included in `Dur`. Note also that it may be possible to edit a segment several times. However, the features are the sums if a segment is edited several times. This implies that the `PostGap` value of a segment may be different from `PreGap` of the next segment, if one of the segments was edited more than once.
Depending on the definition, if the `PostGap` is taken to be pause in the segment, then the number of $\mathtt{pause}$ is the number of typing bursts (TB) plus one,  otherwise the number of pauses is just identical to `TB`.

Based on these considerations, it is possible to compute pause metrics as follows:

PR = (`PreGap` + `TG`) / (`Dur` +1)

$\text{PR} = \frac{\text{PreGap + TG} {\text{Dur} +1}$

$\text{PWR}_S = \frac{TB} {\text{`TokS`}}$

$\text{PWR}_T = \frac{TB} {\text{`TokT`}}$

$$\text{APR} = \frac{TG}{TB} / \frac{TD}/{\text{TokT}} = \frac{TG * TokT}{TB * TD}$$


[^lacruz]:
    - Lacruz et al. (2012): https://aclanthology.org/2012.amta-wptp.3.pdf
    - Lacruz et al. (2014): https://aclanthology.org/2014.amta-wptp.6.pdf
    - Lacruz et al. (2015): https://research-api.cbs.dk/ws/portalfiles/portal/58771005/Michael_Cral_2016_01.pdf


## Typing Inefficiency (InEff)
We adopt the definition of InEff from [TPR-DB version 2.0, p.26](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view), who define typing (in) efficiency for a word, chunk or segment as:

$$ InEff = \frac{\text{number of typed characters}}{\text{length of final translation}} $$

They approximate this in terms of number of insertions and deletions:
 
$$ InEff = \frac{\text{insertions} + \text{deletions}}{\text{insertions - deletions} - 1} $$

A number 1 is added to the denominator to prevent division by 0, for instance in case of postediting when a word or segment remains unchanged.  In the current version we also add 1 to the nominator, so that if no deletions are recorded, the metric will return 1 irrespectively of how many deletions occurred.

$$ InEff = \frac{insertions + deletions + 1}{insertions - deletions - 1} $$

Note that this measure only applies if number of insertions >= number of deletions which ensures that the result >= 1. Otherwise, if there are more deletions than insertions, as might be the case in post-editing, InEff is computed as follows, which provides a number between 0 and 1:

$$ InEff = \frac{1}{deletions} $$


## Gaze measures

Fixations are often considered the basic units of gaze data analysis. Depending on the sample rate of the eyetracken, fixations aggregate several to many gaze sample points $SP$ into one unit, the fixation. In the TPR-DB. the FD table enumerates fixations of a translation session. Fixations can be further aggregated into so-called areas of interest (AOI). The ST and TT tables define each word (i.e., a token) an AOI, the AOIs in the AG table are alignment groups and the SG tabels aggregates fixation data for an entire segment. 

Gaze data can be described on several levels of granularity:

| Level | Unit | Focus | Example Measures |
|---|---|---|---|
| 0 | Sub-fixation | Oculomotor signal | Saccade velocity, pupil size |
| 1 | Single fixation | Local processing |  Number and duration of fixations |
| 2 | Transition between tokens | Sequential processing | First-pass reading time, regression path duration |
| 3 | Gaze path | Local reading strategy | Linear vs. regressive vs. scattered reading  |
| 4 | Cross-trial | Global reader profile | Cluster type, aggregated heatmaps |

The key principle is that each level adds more temporal and spatial context around the fixation event. Currently, the TPR-DB provides some measures for levels 0 - 3 but not for level 4.

### Level 0 — Sub-fixation / Oculomotor Measures
A fixation is a dynamic event that changes in time with respect to the gaze position (micro saccades) and the pupil size. Level 0 measures capture raw oculomotor signal below the fixation level:

- Saccade amplitude, velocity, direction
- Microsaccades during fixation
- [Pupil dilation](#Level 0: Pupilometry) (cognitive load proxy)
- Blink rate and duration

### Level 1 — fixation measures on a word level
Level 1 measures capture what happens at one specific location, in isolation from surrounding context. Fixations are quantified based on their position on the screen (X/Y coordinates), duration, and the character/word/image looked. They reflect early, bottom-up processing:

- First fixation duration: how long the eye rests on first landing in a word
- Single fixation duration: duration when the word receives only one fixation
- Total fixation duration / dwell time: summed time across all fixations on the word
- Number of fixations: how often the word is fixated
- Fixation probability: whether the word was fixated at all


### Level 2 — Transitions between words 
Level 2 measures capture how the eye moves between words. The unit of analysis is the fixation behavior on a word in relation to the neighboring words:

- First-pass reading time: total fixation time of a word before the eye leaves it for the first time
- Regression path duration: time from first fixation until the eye moves *rightward past the word*, including any regressions launched from it
- Go-past time: total time including regressions back into earlier AOIs triggered by this region
- Re-reading time: time spent on second and later passes
- Regression rate in / out: number of regressions into (entering) or out of (leaving) the word
- Refixation rate: probability of making more than one fixation before leaving


### Level 3 — Local reading strategy, gaze path measures
These measures describe the local reading strategy. The unit of analysis is a sequence of fixations and how the eyes move sequentially. 

- Linear reading: sequential, left-to-right, top-to-bottom progression
- Scattered fixations: non-sequential, exploratory, attention-driven
- Regressions / refixations — backward movements, indicating comprehension difficulty or verification
- **Scan path similarity** — comparing one participant's path to another or to an ideal (e.g., Levenshtein distance on AOI sequences)
- **Coverage / revisit rate** — what proportion of AOIs were visited, and how often
- **Reading order deviation** — how much the actual sequence departs from canonical order


### Level 4 — Global gaze behavior
Above the local gaze path level, aggregating across multiple trials or participants:

- Mean gaze behavior per participant (reading speed profile, regression tendency)
- Cluster-based reader types (skimmer vs. careful reader)
- Learning effects across trials


## Level 0: Pupillometry

Pupil measures are a new feature in the TPR-DB 3.0. Pupillometry is the measurement of pupil size and its dynamic changes over time. While the pupil's primary function is the regulation of light intake, it also responds to cognitive and emotional states, which modulates arousal and mental effort. This makes pupil dilation a sensitive, non-invasive proxy for cognitive load.

### The Basic Principle

When a task becomes more mentally demanding, the pupil dilates (widens) slightly — typically by fractions of a millimeter — even under constant lighting conditions. This response is involuntary and continuous, making it useful for tracking moment-to-moment fluctuations in processing difficulty.
In reading research, pupillometry helps reveal where and when comprehension becomes effortful:

- Lexical difficulty: rare or low-frequency words trigger measurable dilation compared to common words
- Syntactic complexity: structurally ambiguous or embedded clauses cause increased load
- Discourse integration: resolving pronouns or bridging inferences produces detectable peaks

Pupil responses are often time-locked to specific words, allowing researchers to pinpoint which linguistic features drive difficulty. However, particular caution needs to be taken in pupillometry since blinks can produce extreme values (outliers) and there are large individual differences: readers with lower working memory capacity tend to show larger and more prolonged dilation responses.

Pupillometry has also become a tool in TPR to assess:

- Source text difficulty: complex syntax, ambiguous terms, or dense domain-specific content.
- Decision points: moments of terminological or stylistic uncertainty show elevated load
- Revision behavior: re-reading and self-correction phases correlate with pupil dilation spikes

Expertise differences — professional translators tend to show more efficient (lower or faster-recovering) pupil responses than novices on equivalent texts, suggesting automatization of sub-processes

### Pupillometric Measures in the TPR-DB

Given the heterogeneous nature of the TPR-DB (different eyetrackers, lighting conditions, sampling rates, etc.) the pupillometric measures are based on change in pupil diameter relative to the median pupil size in each session. 
For each gaze sample (depending on the eyetracker sampling rate) the TPR-DB proceeds in several steps:

1. Average pupil diameter across both eyes for binocular gaze sample data   
2. Compute median pupil diameter across entire session as a baseline  
4. Interpolate over blink gaps, fill edges for sequences of gaze samples  
3. Compute deviation from baseline in various ways for each gaze sample  
5. Compute pupillometric measures for each fixation


Since the pupil size and their changes is specific to every participant, a normalization is required. To address this issue, the TPR-DB computes pupil size baseline as the median pupil diameter for every translation session.

For each gaze sample point $SP$, the TPR-DB 3.0 computes an effective pupil size $SP_p$ as the mean of the left and right pupil diameters when both are available (i.e., diameter $> 0$) for binocular tracking, and otherwise falls back to the available monocular diameter. Each $SP_p$ is then normalised by the session median. In addition, TPR-DB 3.0 computes two measures of dispersion per participant session: a robust median absolute deviation and a standard deviation

- $\mathtt{baseline}  = median(SP_{p})$
- $\mathtt{pupil\_mad} = median(abs(SP_{p} -  \mathtt{baseline}))$
- $\mathtt{pupil\_std} = std(SP_{p})$

An $SP$ can be said to be in a dilated or constricted state relative to the $\mathtt{baseline}$, i.e., a dilation if $SP_{p} > \mathtt{baseline}$ and a constriction if $SP_{p} <= \mathtt{baseline}$

The TPR-DB then computes three sample-level measures 1. percent of change `per` from the baseline, 2. a median-centred z-score `z` and  3. a mean robust z-score `mad`:

1. `per`: $SP_{\mathtt{per}} = 100 * \frac{SP_{AVG} - \mathtt{baseline}}{\mathtt{baseline}}$
2. `z`: $SP_{\mathtt{z}} = \frac{SP_{AVG} -  \mathtt{baseline}}{\mathtt{pupil\_std}}$
3. `mad`: $SP_{\mathtt{mad}} = \frac{SP_{AVG} -  \mathtt{baseline}}{\mathtt{pupil\_mad}}$

For each of the three measures `[per|z|mad]` the TPR-DB produces the following eight commonly used measures in pupillometry research, for each fixation in the FD tables:

| Description | Features in the FD table |
|------|---------| 
| max. pupil size, 95th percentile (peak)| `PUP_[per|mad|z]_max`|
| min. pupil size, 5th percentile (floor) | `PUP_[per|mad|z]_min`|
| mean pupil size | `PUP_[per|mad|z]_mean`|
| standard deviation (SD) | `PUP_[per|mad|z]_mean`|
| Area Under the Curve (AUC) |   `PUP_[per|mad|z]_AUC`|
| time-normalized AUC |   `PUP_[per|mad|z]_AUC_N`|
| time-normalized AUC for constriction |   `PUP_[per|mad|z]_AUC_C`|
| time-normalized AUC for dilation |   `PUP_[per|mad|z]_AUC_D`|

## Level 1 and level 2: Fixation metrics

These metrics have been the in the main focus of research and are documented in the [TPR-DB version 2.0](https://drive.google.com/file/d/1FgOSNcpbjlxdo6MM_jf3Pw5wDS6S9-BB/view) an are FD and FU tables. 

Level 1 metrics include :
- first fixation duration `FFD`,
- Total reading times on the ST word `TrtS`, or on the target word `TrtT`
- Number of fixations  `FixS`, `FixT` 

Level 2 metrics include: 

## Level 3: Gaze Patterns
T

`Dur_L`
`Dur_R`
`Dur_S`
`Dur_N`
`RelDur_L`
`RelDur_R`
`RelDur_S`
`RelDur_N`



## Translation Phases
According to Jakobsen (2011) translation sessions can be separated into an orientation phase (O), a drafting phase (D) and a revision phase (R). Drafting starts with the first keystroke and the time before is defined as the orientation phase. We adopt this definition, even though it may not always be entirely correct. Some translators actually start with testing the keyboard, by typing some characters, and then start the actual orientation phase. We will ignore these cases. According to Jakobsen, drafting ends when the last word has been typed. We operationalize this definitionas follows: 

1. We take the last word in the TT, rather than the translation of the last ST word.
2. Drafting produces at least 50% of the keystrokes in a translation sessions.
3. Drafting proceeds sequentially, i.e., successive keystrokes are no further than [-5 .. +2] word IDs and no more than [-20 .. +10] cursor positions apart. Drafting ends when five (or more) successive keystrokes not in a sequential.

*[IKI]: inter keystroke intervals
*[AUC]: Area Under the Curve: cumulative dilation over a time window
*[SD]: standard deviation: 
*[TPR-DB]: Translation Process Research Database
*[TPR]: Translation Process Research
*[TT]: target text (i.e., translation)
*[ST]: source text 
*[KU]: Keystroke Units; sequences of keystrokes separated by a KU Interruption (KUI)
*[PU]: Production Units; sequences of keystrokes separated by a PU break (PUB)
*[AOI]: Areas of Interest

