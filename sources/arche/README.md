# Clean-up workflow for ARCHE keywords

## 1. Check language tag

The result of the following two actions is stored in `edited/arche_keywords_edit_1_lang.csv`.

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

The result of the following four actions is stored in `edited/arche_keywords_edit_2_spelling.csv`.

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

The result of the following action is stored in `edited/arche_keywords_edit_3_number.csv`.

If a keyword appears only in its plural form, the respective singular form is added to the `singular form` column.
