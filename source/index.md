---
title: Syrinx API Reference

language_tabs:
  - shell: cURL
  - javascript: node.js
  - java

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Syrinx Healthcare IT Platform API.  You can use our API to access endpoints which provide for the collection, aggregation, ingestion and enrichment, and integration of a variety of patient and consumer data sources including EHR patient data.

## Client Libraries

In addition to the REST api, we also offer SDKs in various languages to make it easier for developers to work with the API.  

Supported Languages are:

[javascript](http://github.com)

[java](http://github.com)

## Client Libraries Installation

Please see the instructions for installing your desired client library by selecting the language on the right.

```shell
```

```javascript
npm install syrinx-client
```

```java
j1 placeholder 
```


# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Content-Type: application/json"
  -H "clientToken: <Some Client Token>"
```

```javascript
var syrinx_client = require('syrinx-client');
syrinx_client.config({url: 'http://example.com:8080/rest/v1', clientToken: '123456789'});
```

```java
// Syrinx Client Object
import org.merck.syrinx.client;
SyrinxClient syrinx = new SyrinxClient();

// Configure
String url, clientToken;
syrinx.setUrl(url);
syrinx.setClientToken(clientToken);
```


> Make sure to set clientToken with your API key.

Syrinx uses API keys to allow access to the API.  Syrinx expects for the API key to be included in all API requests to the server in a header that looks like the following:

`clientToken: <Some Client Token>`

<aside class="notice">
You must replace <code>Some Client Token</code> with your API key.
</aside>

# Ingest
The ingest data endpoints are used to add data into the patient data store, any external EMR/EHR systems data can be imported into this patient data store using this API.

## Ingest Formats Accepted
The ingest process uses the Blue Button Parser to convert complex data formats to JSON formats.  Currently 
BlueButton supports the C32, CMS, and CCDA data formats.  The following formats: CCDA, C32, CDA are expressed using the xml data standard.

In addition, if your data files are already in JSON format, they can be ingested directly by the system.

## Ingest a Single File
Submit an ingest job for asynchronous processing for a single file.


```shell
# Ingest a Single File

curl -X "POST" "http://example.com/rest/v1/syrinx/ingest-data" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789" \
	-d $'{
    "dataType" : "filepath",
    "data" : "/home/ubuntu/hit_deploy/sample_ccdas/EMERGE/Patient-1.xml",   
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
```javascript
// Ingest a Single File

var ingestClient = new syrinx_client.ingest(); 

var data = {
    "dataType"     : "filepath", 
	"data"         : "/home/ubuntu/hit_deploy/sample_ccdas/EMERGE/Patient-1.xml",
	"dataFormat"   : "xml",
	"callback_url" : "",              
}

var jobId = ingestClient.ingest(data, function(err, results) {

	if(err) 
		console.err(err);
	else 
		console.log(results);
		
});



// Ingest with Callback

var jobId = ingestClient.ingest(data, function(err, results) {
    
     if(err)
     	console.log(err);
     else 
     	console.log(results);
});


// Ingest without Callback

var jobId = ingestClient.ingest(data);

var results = ingestClient.status(jobId); // This is a blocking call 



```
```java
// Ingest a Single File

import org.merck.syrinx.client.ingest.inputObject;
import org.merck.syrinx.client.ingest.outputObject;
import org.merck.syrinx.client.ingest.inputArray;
import org.merck.syrinx.client.ingest.outputArray;

InputObject<String,Object> data = new InputObject<String, Object>();

{
    "dataType"     : "filepath", 
	"data"         : "/home/ubuntu/hit_deploy/sample_ccdas/EMERGE/Patient-1.xml",   
	"dataFormat"   : "xml",
	"callback_url" : ""                              
}

Ingest ingest = syrinx.ingest();

String jobId = ingest.ingest(data, new StatusListener() {
   @Override 
   public void results(String error, OutputObject results) {
   		if(error != null)
   			System.out.println("Error  " + error);
   }
});
```


### Request

`POST http://example.com/rest/v1/syrinx/ingest-data`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
dataType | Y | "filepath" | Indicates whether the data is embedded in request or resides on server
data | Y | "/path/to/file" | Path to file or directory on the server
dataFormat | Y | "xml:json:rdf" | Format of the file to be ingested
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the job was accepted for processing.

Note, if an invalid ccda format is given, a status code of 412 is returned.

<aside class="success">
Remember to call the Status endpoint to get the status of your submitted job.
</aside>

### Status of Job

```shell
# Get the Status

curl -X "GET" "http://example.com/rest/v1/syrinx/ingest-data/status/JOB1442574337569" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789" \
	-d "{}"
```

```shell

# JSON Result

[
    {
        "message": "COMPLETE",
        "error": [
            "nullFlavor alert:  missing but required street_lines in Address -> Patient -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required text in ResultObservation -> ResultsOrganizer -> resultsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD",
            "nullFlavor alert:  missing but required precondition in medicationActivity -> medicationsSection -> CCD"
        ],
        "jobId": "JOB1442574337569",
        "status": 202,
        "data": []
    }
]
```

```javascript
// Status of Specific Job

// Example of Blocking Call
var results = ingestClient.status(jobId); 


// Example of Asynchronous Call
var results = ingestClient.status(jobId, function(err, results) {
	if(err) 
		console.log(err);
	else 
		console.log(results);
}); 


//Status of All Jobs

var results = ingestClient.status(); // Blocking 

var results = ingestClient.status(function(err, results) {
	if(err) 
		console.log(err);
	else 
		console.log(results);
});
```

```java
// Status of Specific Job

ingest.status(data, new StatusListener() {   
   @Override 
   public void results(String error, OutputObject results) {
   	if(error != null)
   		System.out.println("Error  " + error);
   }
});

IngestObject output = ingest.status(jobId);


// Status of All Jobs

ingest.status(new StatusListener() {  
   @Override 
   public void results(String error, OutputObject results) {
   	if(error != null)
   		System.out.println("Error  " + error);
   }
});

IngestObject output = ingest.status();
```


### Request

`GET http://example.com/rest/v1/syrinx/ingest-data/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | N | "" | The job id of a previously submitted job.  If the job id is not provided, then the endpoint will return the status of all jobs.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.  Errors during parsing are shown in an error array.

Question:  Shouldn't the API be returning Data in the data section?

## Ingest Multiple Files

Ingesting multiple files uses the same API Signature as ingesting a single file.  Instead, the data value will point to a directory on the server where the files are located.

In addition, the data field in the JSON Return will contain the child job ids.  You may call the status endpoint for each of these child jobs.
 
```shell
# Ingest a Directory

curl -X "POST" "http://example.com/rest/v1/syrinx/ingest-data" \
        -H "Content-Type: application/json" \
        -H "clientToken: 123456789" \
        -d $'{
    "dataType" : "filepath",
    "data" : "/home/ubuntu/hit_deploy/sample_ccdas/demo/",
    "dataFormat"   : "xml",
    "callback_url" : ""
}'
```
```shell
# JSON Result

{
    "jobId": "JOB1442355494309",
    "message": "Accepted",
    "status": 202
}


# Get the Status

curl -X "GET" "http://example.com/rest/v1/syrinx/ingest-data/status/JOB1442574337569" \
        -H "Content-Type: application/json" \
        -H "clientToken: 123456789" \
        -d "{}"

# JSON Result

[
    {
        "message": "COMPLETE",
        "error": [],
        "jobId": "JOB1442608173742",
        "status": 202,
        "data": [
            "JOB1442608173742_001",
            "JOB1442608173742_011",
            "JOB1442608173742_021",
            "JOB1442608173742_031"
        ]
    }
]




```

```javascript
```

```java
```

## Ingest Embedded Data
### Request

`POST http://example.com/rest/v1/syrinx/ingest-data`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
dataType | Y | "embedded" | Indicates whether the data is embedded in request or resides on server
data | Y | "" | String representation of actual contents
dataFormat | Y | "xml:json:rdf" | Format of the file to be ingested
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the job was accepted for processing.

## Status of Ingest

### Status of All Jobs
`GET http://example.com/rest/v1/syrinx/ingest-data/status`

### Status of Job
#### Request

`GET http://example.com/rest/v1/syrinx/ingest-data/status/{jobId}`

#### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | N | "" | The job id of a previously submitted job.

#### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.  Errors during parsing are shown in an error array.

### Status by Job State
GET http://example.com/rest/v1/syrinx/ingest-data?status={value}

where {value} is one of COMPLETE, FAILED, [COMPLETE_WITH_ERROR] <= Check this

# Standardize

# Filter

# Aggregate
