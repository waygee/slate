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

This endpoint will be able to map to and from the following set of standards:

* From ICD-9 to SNOMED-CT
* From ICD-10 to SNOMED-CT
* From CPT to LOINC

## Standardize All Records

```shell
# Standardize All Records

curl -X "POST" "http://example.com/rest/v1/syrinx/standardize-data" \
        -H "Content-Type: application/json" \
        -H "clientToken: 123456789" \
        -d $'{
    "dataType" : "query",
    "data" : "",
    "dataFormat"   : "xml",
    "callback_url" : ""
}'
```
```shell
# JSON Result

{
    "jobId": "JOB1442337673507",
    "message": "Accepted",
    "status": 202
}
```

### Request

`POST http://example.com/rest/v1/syrinx/standardize-data`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
dataType | Y | "query" | Indicates whether the data is embedded or is a MongoDB query.
data | Y | "" | Empty query runs through entire database.  Only query that is supported right now is empty.
dataFormat | Y | "xml" | 
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the job was accepted for processing.

## Status of Standardize

```shell
# Get the Status

curl -X "GET" "http://example.com/rest/v1/syrinx/standardize-data/status/JOB1442862119598" \
        -H "Content-Type: application/json" \
        -H "clientToken: 123456789" \
        -d "{}"
```
```shell
# JSON Result
[
    {
        "message": "COMPLETE",
        "status": 202,
        "data": []
    }
]
```
### Request

`GET http://example.com/rest/v1/syrinx/standardize-data/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | Y | "" | The job id of a previously submitted job.  If the job id is not provided, then the endpoint will return the status of all jobs.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.
