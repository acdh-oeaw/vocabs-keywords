# Clean-up workflow for ARCHE keywords


This directory contains a formal and semantic revision of the values used for three properties in the [ARCHE schema](https://github.com/acdh-oeaw/arche-schema/blob/4c73c172df9b083e5c1d6c841875873e20c22202/acdh-schema.owl) as of 2022-01-29: `hasAppliedMethod`, `hasCategory`, `hasSubject`. The results of this revision can be used to **clean up** the values assigned to these properties, as well as to prepare **controlled vocabularies** to be used when filling in these properties or for other purposes (*e.g.*, a more general DH keywords vocabulary).

The following worflow was used to analyze and revise all these values, also referred to as **keywords**.

## 1. Get all keywords

The file `arche_keywords_raw.csv` contains all values for the three above-mentioned properties, as they currently stand in the repository, with this additional information:
* `lang`: the language tag associated with the value
* `property`: the property for which this value is used
* `count`: number of times the value is used throughout ARCHE
* `id`: the internal ARCHE identifier assigned to the value. This applies only to values coming from property `hasCategory`, which are entities and not just strings.

Some values might appear more than once, for the following reasons:

* they might occur with different language tags, or might not have a language tag in one of their occurrences (*e.g.*, `3D scanning`: once with no language tag, once with `en`)
* they might be assigned to different properties
* one of the occurrences might contain a trailing space

All these duplicates will be treated in the following revision steps.

Entities from the `hasCategory` property are listed twice, once with the German label, once with the English one, *e.g.* `Bild` and `Image`. These double labels can be easily spotted by looking at the `id` column, which has the same value for both keywords.

## 2. Pre-process

Before proceeding with further steps, it is necessary to apply some basic pre-processing steps, in order to faciliate the following analysis. These pre-processing steps are described by the following colums included in the `arche_keywords_raw.csv` file:

* `delete`
* `separate`
* `remove_trailing_space`

Here follows a more detailed illustration of these steps.

### 2.1 Delete too long values

One value for property `hasSubject` cannot be actually considered a keyword. It is instead a long description of the related collection:

```
VOICE, the Vienna-Oxford International Corpus of English, is a one-million-word corpus of naturally-occurring, non-scripted, face-to-face interactions carried out using English as a lingua franca (ELF), i.e. English used as a common means of communication among speakers from different first-language backgrounds. The interactions recorded and transcribed are complete speech events from different domains (educational, leisure, professional) and represent different speech event types (conversation, interview, meeting, panel, press conference, question-answer session, seminar discussion, service encounter, working group discussion, workshop discussion).
```

This value is marked with `yes` in the `delete` column and thus deleted.

- [ ] Extraction of specific keywords from this description might be discussed in the ARCHE curation team.

### 2.2 Separate values combining multiple keywords

Some values contain multiple keywords combined through punctuation or conjunctions. These are divided into separate values and will be treated as such in the following analysis. All these values are marked with `yes` in the `separate` column.

If a value corresponds to an existing one after the separation of the combined keywords, the value is listed only once in the processed table and the counts of the multiple instances are combined. Here follows a table illustrating all relevant values and the resulting keywords.

|        | value                                                        | lang | separator       | number of keywords | resulting keywords              |
| ------ | ------------------------------------------------------------ | ---- | --------------- | ------------------ | ------------------------------- |
| **1**  | `archaeological excavation, pottery documentation, photography` | `en` | comma           | 3                  | `archaeological excavation`     |
|        |                                                              |      |                 |                    | `pottery documentation`         |
|        |                                                              |      |                 |                    | `photography`                   |
| **2**  | `Archaeological excavation, pottery documentation, photography.` | `en` | comma           | 3                  | `Archaeological excavation`     |
|        |                                                              |      |                 |                    | `pottery documentation`         |
|        |                                                              |      |                 |                    | `photography.`                  |
| **3**  | `Berliner Tageblatt, Kerr, Wolff`                            | `de` | comma           | 3                  | `Berliner Tageblatt`            |
|        |                                                              |      |                 |                    | `Kerr`                          |
|        |                                                              |      |                 |                    | `Wolff`                         |
| **4**  | `data management, data curation`                             | `en` | comma           | 2                  | `data management`               |
|        |                                                              |      |                 |                    | `data curation`                 |
| **5**  | `Die Stunde, Békessy`                                        | `de` | comma           | 2                  | `Die Stunde`                    |
|        |                                                              |      |                 |                    | `Békessy`                       |
| **6**  | `image creation (scanning, photography)`                     |      | comma, brackets | 3                  | `image creation`                |
|        |                                                              |      |                 |                    | `scanning`                      |
|        |                                                              |      |                 |                    | `photography`                   |
| **7**  | `Mappe, M.S.MAP `                                            | `de` | comma           | 2                  | `Mappe`                         |
|        |                                                              |      |                 |                    | `M.S.MAP `                      |
| **8**  | `Neolithic Greece and Anatolia`                              | `en` | `and`           | 2                  | `Neolithic Greece`              |
|        |                                                              |      |                 |                    | `Neolithic Anatolia`            |
| **9**  | `Neolithic Greece and Anatolia`                              |      | `and`           | 2                  | `Neolithic Greece`              |
|        |                                                              |      |                 |                    | `Neolithic Anatolia`            |
| **10** | `OCR, XML/TEI structural and semantic annotation`            |      | comma, `and`    | 3                  | `OCR`                           |
|        |                                                              |      |                 |                    | `XML/TEI structural annotation` |
|        |                                                              |      |                 |                    | `XML/TEI semantic annotation`   |
| **11** | `pottery documentation, inked drawings`                      | `en` | comma           | 2                  | `pottery documentation`         |
|        |                                                              |      |                 |                    | `inked drawings`                |
| **12** | `pottery documentation, inked drawings`                      |      | comma           | 2                  | `pottery documentation`         |
|        |                                                              |      |                 |                    | `inked drawings`                |
| **13** | `Schober, 15. Juli 1927`                                     | `de` | comma           | 2                  | `Schober`                       |
|        |                                                              |      |                 |                    | `15. Juli 1927`                 |
| **14** | `site and monument`                                          | `en` | `and`           | 2                  | `site`                          |
|        |                                                              |      |                 |                    | `monument`                      |
| **15** | `Vereins- und Versammlungsfreiheit`                          | `de` | `und`           | 2                  | `Vereinsfreiheit`               |
|        |                                                              |      |                 |                    | `Versammlungsfreiheit`          |
| **16** | `XML/TEI structural and semantic annotation`                 |      | `and`           | 2                  | `XML/TEI structural annotation` |
|        |                                                              |      |                 |                    | `XML/TEI semantic annotation`   |

Some values might be discussed:

- [ ] Is `OCR, XML/TEI structural and semantic annotation` made of 2 or 3 values? That is, apart from `OCR`, should we create two separate keywords `XML/TEI structural annotation` and `XML/TEI semantic annotation` or just a unique `XML/TEI structural and semantic annotation`?
- [ ] We might keep in mind that `image creation` is presented here as an overarching category for both `scanning` and `photography`, so we might model these last two keywords as 'subconcepts' of `image creation` when developing a SKOS vocabulary.
- [ ] Is it ok to divide `Neolithic Greece and Anatolia` into two separate keywords? Or do they rather belong together to indicate an historical macro-region?
- [ ] A further value, `professional research and science` (not included in the table above), might be considered as made up of two alternate labels (`professional research` ~ `professional science`), based on similar keywords (*e.g.*, `professional research/science conversation`). Or is it rather two separate concepts?
- [ ] The same for `Marken-/Namensrechtsverletzung` (not included in the table above).
- [ ] The same for `Vereins- und Versammlungsfreiheit`, which is however included in the table above, because it seems to be composed of two separate concepts rather than two different labels.

Also, regarding the `hasCategory` vocabulary:

- [ ] Value `Workflow, Arbeitsablauf` for property `hasCategory` can be separated into two different labels (according to the SKOS ontology: one `prefLabel` and one `altLabel`, both for German language). But this will entail a change in the SKOS representation of this vocabulary.

### 2.3 Remove trailing space

Some values present a trailing space, which might also lead to "apparent" duplicates in the list. All these values are marked with `yes` in the column `remove_trailing_space`. If a value corresponds to an existing one after the removal of the trailing spaces, the value is listed only once in the processed table and the counts of the multiple instances are combined. The following three values are affected:

* `Mappe, M.S.MAP `
* `D.J.STZ `
* `Graf Leo Thun-Hohenstein `

## 3. Correct formal aspects

All the following changes are indicated in the file `arche_keywords_processed.csv`. Here follows a description of the specific actions and the related columns in the file.

### 3.1 Check language tag

#### 3.1.1 Include a language tag where this is missing

If a term has **no** language tag in the `lang` column, **a language tag is added** to the `lang_correct` column.

The language tag `und` is assigned to **proper names** of places and persons. In this case, the note `proper name` is added to the `lang_correct_comments` column.

If the name of an **organization** or a **product** (_e.g._, a newspaper) is clearly in a certain language, such as `Austrian Press Agency` and `Berliner Tageblatt`, then the appropriate language tag is assigned. Otherwise, the language tag `und` is assigned, just as for proper names of places and persons.

The language tag `und` is also assigned to **abbreviations used in/suitable for multiple languages** (_e.g._, `TLS` = `terrestrial laser scanning` [English] / `terrestriches Laserscanning` [German]); in this case, the note `non-determinable abbrevation` is added to the `lang_correct_comments` column. The language tag `und` is also assigned when an abbreviation comes from words in multiple languages, such as `CIDOC CRM` (= `Comité International pour la Documentation` [French] + `Conceptual Reference Model` [English]); in this case, the note `mixed abbrevation` is added to the `lang_correct_comments` column.

When an abbreviation is clearly the result of **words coming from a specific language** (*e.g.*, `GPR` from "Ground-penetrating radar"), then the appropriate language tag (in the example, `en`) is assigned.

In some cases, it is **ambiguous** to which language a keyword belongs (for example, `Edition`, which might be English or German). In these cases, it is examined in which context the keyword appears: _e.g._, if the keyword is always used along other keywords in German when describing resources in ARCHE, then it is assumed that this keyword too is in German.

#### 3.1.2 Correct existing language tags

If a term has a wrong language tag in the `lang` column, **the correct language tag is assigned** in the `lang_correct` column.

All criteria exposed in the previous section apply here too.

#### 3.1.3 Open questions

- [ ] How should we treat abbreviations from [Legal Kraus](https://id.acdh.oeaw.ac.at/legalkraus) (*e.g.*, `C.K.KOR`)? Can we consider them all `de`? They are all abbreviations of other keywords used for the same resources (in the case of `C.K.KOR`, `Korrespondenz`)
- [ ] The same goes for other cases of `und`: are we really going to add language tags? Or should we just leave these keywords without language tag (it is not necessary to add one)

### 3.2 Correct spelling

#### 3.2.1 Check if there are any orthographic mistakes

When a keyword contains a form that is orthographically inadmissible in its language, the correct form is added to the `value_correct` column.

#### 3.2.2 Adapt to orthographic conventions

The following orthographic conventions are followed:
* initial lowercase letters unless strictly necessary (this applies, for example, to most of the English words and to German adjectives, even if they appear as the first word in the label)
    * however, when a noun refers to a specific object or event (_e.g._, the `Congress of ...`), it is written with an initial capital letter
* UK spelling for English words
* modern (post-1996) German orthography
* all words written in full form (for example, `deutschösterreichisch` instead of `deutschösterr.`; this, however, does not apply to acronyms like `OCR`)
* proper names in full form (_e.g._, `Alfred Kerr` instead of `Kerr`)

If a keyword does not respect any of these orthographic conventions, the correct form is added to the `value_correct` column.

#### 3.2.3 Open questions

- [ ] In some cases, it was not clear how to deal with capitalization. In particular, `Second world war` --> `Second World War`? And `Viennese Dialectological School` --> `Viennese dialectological school`?
- [ ] As regards `unknown adm`, we might need more information about the use of this tag in the [related resource](https://id.acdh.oeaw.ac.at/histogis/europe-stateborders-1913-1__1913-05-31_1913-08-10/formerly-ottoman-territories-ceded-to-the-balkan-league__1913-05-31_1913-08-10).

### 3.3 Include singular form

If a keyword appears only in its plural form, the respective singular form is added to the `singular_form` column.

The inclusion of the singular form does not mean that it will be used as a preferred way to refer to the concept. In some cases, actually, the use of the singular form might not make much sense (for example, `Monarchenkongresse` is an established concept in its plural form).

It was avoided to include singular forms for keywords `data` or `Daten` or compound words which have `data` or `-daten` as their heads (*e.g.*, `excavation data`).

### 3.4 Separate qualifiers

In some cases, a more specific qualifier is added to the keyword, to better explain its scope and meaning, for example in `Beleg (jur.)`. In such cases, the main term is recorded in the column `main_part`, while the added term in the column `qualifier`. This way, it is easier to decide whether to include or exclude the additional qualifier for future analyses.

### 3.5 Separate variants

In some cases, a value may actually contain different ways of expressing the same concept, usually separated by a slash (`Druck mit handschriftliche Annotationen/Notaten/Notizen`) or included between brackets (for example, when an abbreviation is mentioned after the full form: `Structure from Motion (SfM)`). In these cases, one of the labels (preferably, the most common one) is included in the `main_label` column, while the other forms are listed under `variants`.

- [ ] `Markenrechtsverletzung` or `Markenverletzung`?

### 3.6 Change order

In cases such as `Geist (moderner)`, the adjective was put after the noun to facilitate indexing; however, here the order `moderner Geist` is preferred and recorded in the column `swap_order`.

## 4. Semantic analysis

The file [arche_keywords_previous_analyses.csv](./arche_keywords_previous_analyses.csv) contains a comparison of all the previous analyses performed by Martin Kirnbauer, Daniel Schopper and Seta Štuhec on the semantics of the keywords. The different chronological steps in which these analyses were performed are indicated by the relevant date in the column header (*e.g.*, `2022-07-15`).

All the relevant analyses and comments contained in the folder [hasSubject](https://oeawacat.sharepoint.com/:f:/r/sites/ACDH-CH_i_dsie/Shared Documents/DataPreservation/generalCurationTasks/hasSubject?csf=1&web=1&e=hMujcl) on SharePoint were included (access restricted to ÖAW).

In particular:

* `hasSubject_2022_05_03.txt`: only contains a list of the keywords; not considered
* `hasSubject_2022_07_15.xlsx`: first analysis performed by Martin Kirnbauer. The results were included in the following columns:
  * `2022-07-15: Nichtbeachtung weil`
  * `2022-07-15: Anmerkung`
  * (The column `Aufnahme weil` of the original spreadsheet was not included since it contained no values)
* `hasSubject_2022_07_15_Vorgehen.docx`: accompanying Word document where Martin Kirnbauer illustrates the criteria used and proposes a semantic classification of the keywords. Included in:
  * `2022-07-15: Klassifizierung`
  * `2022-07-15: Comments by Seta` (further comments added to the document by Seta Štuhec)
* `hasSubject.xlsx`: analysis performed by Daniel Schopper (last modification of the file on 2022-09-15). Results included in the columns:
  * `2022-09-15: Category`
  * `2022-09-15: Action` 
  * `2022-09-15: Note DS`
* `hasSubject_2022_09_23.xslx`: further analysis by Seta Štuhec. Results included in the columns:
  * `2022-09-23: Nichtbeachtung weil`
  * `2022-09-23: Anmerkung`
  * (The column `Aufnahme weil` of the original spreadsheet was not included since it contained no values)
  * The file also contains a proposal for an archaeological vocabulary, contained in the sheet `hasSubject_ARCHAEOLOGY`. The results were included in the columns:
    * `2022-09-23 hasSubject_ARCHAEOLOGY: include?` (value `archaeology` for terms that should be included in the archaeological vocabulary)
    * `2022-09-23 hasSubject_ARCHAEOLOGY: notes`
* `2022-09-29_ARCHEterms.csv`: further analysis by Seta Štuhec. Results included in columns:
  * `2022-09-29: ARCHEgroup`
  * `2022-09-29: inARCHEmappedAs`
  * `2022-09-29: skos:match`
  * `2022-09-29: Vocabulary`
  * `2022-09-29: Notes`

* `2022-09-29_ARCHEterms.xlsx`: similar to the previous CSV file, but with some changes
  * `2022-09-29_xlsx: ARCHEgroup`
  * `2022-09-29_xlsx: skos:match`
  * `2022-09-29_xlsx: Vocabulary`
  * `2022-09-29_xlsx: Notes`
