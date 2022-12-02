# Clean-up workflow for ARCHE keywords

## 1. Check language tag

The result of the following two actions is stored in [arche_keywords_edit_1_lang.csv](./arche_keywords_edit_1_lang.csv).

### 1.1 Include a language tag where this is missing

If a term has **no** language tag in the `lang` column, **a language tag is added** to the `correct_lang` column.

The language tag `und` is assigned to **proper names** of places and persons. In this case, the note `proper name` is added to the `notes_lang` column.

If the name of an **organization** or a **product** (_e.g._, a newspaper) is clearly in a certain language, such as `Austrian Press Agency` and `Berliner Tageblatt`, then the appropriate language tag is assigned. Otherwise, the language tag `und` is assigned, just as for proper names of places and persons.

The language tag `und` is also assigned to **abbreviations used in/suitable for multiple languages** (_e.g._, `TLS` = `terrestrial laser scanning` [English] / `terrestriches Laserscanning` [German]). The language tag `und` is also assigned when an abbreviation comes from words in multiple languages, such as `CIDOC CRM` (= `Comité International pour la Documentation` [French] + `Conceptual Reference Model` [English]). In the case of an abbreviation, the term `abbreviation` is also added to the `notes_lang` column.

A special case is represented by the numerous abbreviations coming from the [Downed Allied Air Crew Database Austria](https://hdl.handle.net/21.11115/0000-000D-CA69-A) dataset. These are assigned the language tag `und` and the note `DAACDA` is added to the `notes_lang` column, so that they can be easily excluded from future analyses if desired.

In some cases, it is **ambiguous** to which language a term belongs (for example, `Edition`, which might be English or German). In these cases, it is examined in which context the term appears: _e.g._, if the term is always used along other terms in German when describing resources in ARCHE, then it is assumed that this term too is in German.

### 1.2 Correct existing language tags

If a term has a wrong language tag in the `lang` column, **the correct language tag is assigned** in the `correct_lang` column.

## 2. Correct spelling

The result of the following four actions is stored in [arche_keywords_edit_2_spelling.csv](./arche_keywords_edit_2_spelling.csv).

### 2.1 Remove trailing spaces

If a keyword has a trailing space, the value `yes` is added to the `remove trailing space` column.

No keywords have leading spaces.

### 2.2 Check if there are any orthographic mistakes

When a keyword contains a form that is orthographically inadmissible in its language, the correct form is added to the `correct to` column.

### 2.3 Adapt to orthographic conventions

The following orthographic conventions are followed:
* initial lowercase letters unless strictly necessary (this applies, for example, to most of the English words and to German adjectives, even if they appear as the first word in the label)
    * however, when a noun refers to a specific object or event (_e.g._, the `Congress of ...`), it is written with an initial capital letter
* UK spelling for English words
* modern German orthography
* all words written in full form (for example, `deutschösterreichisch` instead of `deutschösterr.`; this, however, does not apply to acronyms like `OCR`)
* proper names in full form (_e.g._, `Alfred Kerr` instead of `Kerr`)
* comma instead of conjunction `and` when two keywords are combined in the same label

If a keyword does not respect any of these orthographic conventions, the correct form is added to the `correct to` column.

### 2.4 Include orthographic variants

As far as possible, spelling variants are included in the `variants` column. These include:
* US spelling for English words
* (non-)hyphenated forms (_e.g._, `deutsch-österreichisch` for `deutschösterreichisch`)
* full forms of acronyms

## 3. Include singular form

The result of the following action is stored in [arche_keywords_edit_3_number.csv](./arche_keywords_edit_3_number.csv).

If a keyword appears only in its plural form, the respective singular form is added to the `singular form` column.

## 4. Separate combined keywords

The result of the following actions is stored in [arche_keywords_edit_4_separate.csv](./arche_keywords_edit_4_separate.csv).

In some cases, a keyword may actually contain several keywords, usually separated by a comma (but also a slash or brackets), such as in `data management, data curation`. In these cases, the column `separate` is filled with the value `yes`. Some of the additional keywords might be moved to the `variants` column as different ways of expressing the same concept.

In other cases, a more specific qualification is added to the keyword, to better explain its scope and meaning, for example in `Beleg (jur.)`. In such cases, the main term is recorded in the column `main part`, while the added term in the column `qualification`. This way, it is easier to decide whether to include or exclude the additional qualification for future analyses.

There are also a few cases where the two elements of a keywords need to be combined together. In cases such as `Geist (moderner)`, the adjective was put into brackets to facilitate indexing; however, here the whole form (`moderner Geist`) is preferred and recorded in the column `combine`.

## 5. Additional steps

The result of the following actions is stored in [arche_keywords_edit_5_additional.csv](./arche_keywords_edit_5_additional.csv).

German equivalents for the keywords coming from property `hasCategory` are added.

Keyword with `id` = `011` is deleted, as it contains a long text regarding the VOICE corpus.

At this stage, some rows have the same `id`, since they originate from the separation of single rows that contained more than one keyword, or are the German equivalents of the `hasCategory` keywords. IDs will be adjusted in the next stage.

## 6. Remove duplicates

The result of the following actions is stored in [arche_keywords_edit_6_final.csv](./arche_keywords_edit_6_final.csv).

Duplicates are removed.

All rows are assigned a new unique three-digit identifier in column `id`.