<!---
Group:drug
Name:D10 Find ingredient by drug class
Author:Patrick Ryan
CDM Version: 5.0
-->

# D10: Find ingredient by drug class

## Description
This query is designed to extract all ingredients that belong to a therapeutic class. The query accepts a therapeutic class concept ID as the input and returns all drugs that are included under that class.
Therapeutic classes could be obtained using query  [D02](http://vocabqueries.omop.org/drug-queries/d2) and are derived from one of the following:

- Enhanced Therapeutic Classification (FDB ETC), VOCABULARY_ID = 20
- Anatomical Therapeutic Chemical classification (WHO ATC), VOCABULARY_ID = 21

– NDF-RT Mechanism of Action (MoA), Vocabulary ID = 7, Concept Class = 'Mechanism of Action'

– NDF-RT Physiologic effect (PE), Vocabulary ID = 7, Concept Class = 'Physiologic Effect'

– NDF-RT Chemical Structure, Vocabulary ID = 7, Concept Class = 'Chemical Structure'

-  VA Class, VOCABULARY_ID = 32

## Query
```sql
SELECT  c.concept_id    ingredient_concept_id,
        c.concept_name  ingredient_concept_name,
        c.concept_class_id ingredient_concept_class,
        c.concept_code  ingredient_concept_code
 FROM   @vocab.concept          c,
        @vocab.concept_ancestor ca
 WHERE  ca.ancestor_concept_id = 21506108
   AND  c.concept_id           = ca.descendant_concept_id
   AND  c.vocabulary_id        = 'RxNorm'
   AND c.concept_class_id = 'Ingredient'
   AND  getdate() BETWEEN c.valid_start_date AND c.valid_end_date;
```

## Input

|  Parameter |  Example |  Mandatory |  Notes |
| --- | --- | --- | --- |
|  Therapeutic Class Concept ID |  21506108 |  Yes | Concept ID for 'ACE Inhibitors and ACE Inhibitor Combinations' |
|  As of date |  Sysdate |  No | Valid record as of specific date. Current date – sysdate is a default |



## Output

|  Field |  Description |
| --- | --- |
|  Ingredient_Concept_ID |  Concept ID of ingredient included in therapeutic class |
|  Ingredient_Concept_Name |  Name of ingredient concept included in therapeutic class |
|  Ingredient_Concept_Class |  Concept class of ingredient concept included in therapeutic class |
|  Ingredient_Concept_Code |  RxNorm source code of ingredient concept |

## Sample output record

|  Field |  Value |
| --- | --- |
|  Ingredient_Concept_ID |  1308216 |
|  Ingredient_Concept_Name |  Lisinopril |
|  Ingredient_Concept_Class |  Ingredient |
|  Ingredient_Concept_Code |  29046 |



## Documentation
https://github.com/OHDSI/CommonDataModel/wiki/
