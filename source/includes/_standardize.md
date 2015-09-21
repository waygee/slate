# Standardize

The Standardize endpoint maps the vocabularies in the source data to
standard vocabularies.

Data mapping involves taking raw HL7-CCDA and map sections to common
vocabularies. An overview of the CCDA sections are given in:

[clinical document sections](http://developers.amida-tech.com/document_model.html)


A great overview table of standard vocabularies and the data elements that are coded are given at:

[Vocabulary Data Elements table](http://library.ahima.org/xpedio/groups/public/documents/ahima/bok1_033474.hcsp?dDocName=bok1_033474)

 The standard vocabularies to be mapped to are
given below:

 The standard vocabularies to be mapped to are
given below:

|C-CDA section |        Data Elements   |Standard Vocabulary|
|-------------|:-------------:|---------------:|
|Demographics           |Demographics | N/A                         |
|Allergies               | Observations->Allergen, | SNOMED CT|
|Allergies               | Observations->Reactions->Reaction, | SNOMED CT|
|Allergies               | Observations->Reaction->Severity, | SNOMED CT|
|Encounters     |encounter                    | SNOMED CT |
|                |      findings->value                    |  SNOMED CT|
|Immunizations  |product->product       |LOINC|
|       |administration->route  |SNOMED CT|
|       |instructions->code     |SNOMED CT|
|Medications    |Product->product       |RXNORM |
|       |Administration->route  |SNOMED CT|
|       |Administration->form   |SNOMED CT|
|       |Precondition->value    |SNOMED CT|
|       |Indication->value      |SNOMED CT|
|Problems       |Problem (illness history)|     SNOMED CT|
|Procedures     |Procedure      |SNOMED CT|
|       |specimen       |SNOMED CT|
|Results|       results->result (lab results)|  LOINC|
|Social history |Social history entry|  N/A|
|Plan of care   |plan   |SNOMED CT|
|Vitals |vitals (height, weight, etc)   |LOINC|
|Payers |policy |HL7 role |
|       |guarantor      |HL7 role|
|       |participant    |HL7 role|
|       |performer      |HL7 role|
|       |authorization->procedure|      SNOMED CT|
