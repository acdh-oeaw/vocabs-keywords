# Clean-up workflow for ARCHE keywords

## 1. Check language tag

The result of the following two actions is stored in `edited/arche_keywords_edit_1_lang.csv`.

### 1.1 Include a language tag where this is missing

If a term has no language tag in the `lang` column, **a language tag is added** to the `correct_lang` column.

No language tags are assigned to **proper names** (places and persons) nor to **abbreviations used in/suitable for multiple languages** (_e.g._, `TLS` = `terrestrial laser scanning` [English] / `terrestriches Laserscanning` [German]).

Also, when an abbreviation comes from words in multiple languages, such as `CIDOC CRM` (= `Comité International pour la Documentation` [French] + `Conceptual Reference Model` [English]), no language tag is assigned.

If the name of an **organization** or a **product** (_e.g._, a newspaper) is clearly in a certain language, such as `Austrian Press Agency` and `Berliner Tageblatt`, then the relevant language tag is assigned. Otherwise, it is handled just like proper names of persons and places (see above).

In some cases, it is **ambiguous** to which language a term belongs (for example, `Edition`, which might be English or German). In these cases, it is examined in which context the term appears: _e.g._, if the term is always used along other terms in German when describing resources, then it is assumed that this term too is in German.

### 1.2 Correct existing language tags

If a term has a wrong language tag in the `lang` column, **the correct language tag is assigned** in the `correct_lang` column.

If a certain term already has a language tag assigned to it, but **should not have one** (according to the rules in **1.1**), this tag is marked for deletion by inserting the keyword `delete` in the `correct_lang` column.
