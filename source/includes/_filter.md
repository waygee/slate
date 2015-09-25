# Filter
Filters allow you to retrieve records from the Syrinx data store based on a specific filter criteria.  You are able to indicate specific fields that should be returned using the projection.  You will call the Status endpoint to actually get the data as a result of running the filter.  In addition, filters may be stored and used by other API calls such as Aggregate and Scoring.

## Create Filter
```shell
# Create a Filter

curl -X "POST" "http://example.com/rest/v1/syrinx/c" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789" \
	-d $'{     
	  "filterMetadata": "{\'Filter Name\': \'Vitals of All Patients\'}",
	  "filter": "{}",
	  "projection": "data.vitals",
	  "storedFilter": "true",
	  "callback_url": "" 
}'

# JSON Result

{
    "jobId": "JOB1442862119598",
    "message": "Accepted",
    "status": 202
}

```
### Request

`POST http://example.com/rest/v1/syrinx/filter`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
filterMetadata | N | "{}" | A JSON object with key value pairs representing meta data that can be stored with the filter.
filter | Y | "{}", "[]", "filterId" | Can be a JSON object, array, or string.  JSON Objects conform to MongoDB query rules. If an array is passed, the API would expect either a JSON object or a string as part of the array. If the array element is a string then it is treated as filterId whereas JSON Object is treated as filter condition.  Please see specific examples of filter parameter below. 
projection | N | "" | Limits the fields to return for all matching documents.  Projection is specified as a comma separated list of fields.
storedFilter | Y | "true:false" | If the filter is to be used in other api calls (like Aggregate or Scoring), set this to "true". 
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the filter was accepted for processing.

### Example Usages of Filter Parameter
*Example 1*

filter = {"data.demographics.gender":"Male"}

*Example 2*

filter = [{"data.demographics.gender":"Male"}, {"data.demographics.race":"White"}]

*Example 3*

filter = ["a0c6678b-b8ec-4e1e-befe-a9f05fa91d83", "4feabc09-01e7-4762-9443-a59d7b1bf4d3"] // List of filterIds

*Example 4*

filter = [{"data.demographics.gender":"Male"}, "4feabc09-01e7-4762-9443-a59d7b1bf4d3", {"data.demographics.race":"White"}, "FilterId"]

## Get Status of Filter

```shella

# Get the Status

curl -X "GET" "http://example.com/rest/v1/syrinx/filter/status/JOB1442862119598" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"


# JSON Result
# Data has been shortened for the example

[
    {
        "message": "COMPLETE",
        "status": 202,
        "data": [
            {
                "refById": true,
                "documents": [
                    {
                        "_id": {
                            "$oid": "5602f523cd7f8926a179e7d6"
                        },
                        "data": {
                            "vitals": [
                                {
                                    "unit": "kg/m2",
                                    "vital": {
                                        "name": "BMI",
                                        "code_system_name": "LOINC",
                                        "code": "39156-5"
                                    },
                                    "identifiers": [
                                        {
                                            "identifier": "c6f88321-67ad-11db-bd13-0800200c9a66"
                                        }
                                    ],
                                    "status": "completed",
                                    "value": 31.3,
                                    "date_time": {
                                        "point": {
                                            "precision": "day",
                                            "date": "2010-03-31T00:00:00.000Z"
                                        }
                                    }
                                },
                                {
                                    "unit": "/min",
                                    "vital": {
                                        "name": "Heart Rate",
                                        "code_system_name": "LOINC",
                                        "code": "8867-4"
                                    },
                                    "identifiers": [
                                        {
                                            "identifier": "c6f88321-67ad-11db-bd13-0800200c9a66"
                                        }
                                    ],
                                    "status": "completed",
                                    "value": 88,
                                    "date_time": {
                                        "point": {
                                            "precision": "day",
                                            "date": "2010-05-28T00:00:00.000Z"
                                        }
                                    }
                                }
                            ]
                        }
                    }
                ],
                "filter_id": "f8b95b1a-4511-4007-97d3-7700ee4e58e1",
                "metadata": {
                    "Filter Name": "All Patients"
                }
            }
        ]
    }
]
```

### Request

`GET http://example.com/rest/v1/syrinx/filter/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | Y | "" | The job id of a previously submitted job.  If the job id is not provided, then the endpoint will return the status of all jobs.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.

## Get Filters

```shell
# Get Stored Filters

curl -X "GET" "http://example.com/rest/v1/syrinx/filter?storedFilter=true" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"

# JSON Result

{
    "jobId": "JOB1443136920365",
    "message": "Accepted",
    "status": 202
}


# Get the Status

curl -X "GET" "http://example.com/rest/v1/syrinx/filter/status/JOB1443136920365" \
    -H "Content-Type: application/json" \
    -H "clientToken: 123456789"

# JSON Result 

[
    {
        "message": "COMPLETE",
        "status": 202,
        "data": [
            {
                "filters": [
                    {
                        "_id": {
                            "$oid": "56048592cd7f89537e56bd6a"
                        },
                        "projection": "{}",
                        "filter_id": "ad9f321c-1d56-4eba-bd78-4ed43c883165",
                        "filter": "{}"
                    },
                    {
                        "_id": {
                            "$oid": "56048595cd7f89537e56bd6c"
                        },
                        "projection": "{}",
                        "filter_id": "db60216d-d186-4493-ac4d-442ef2355714",
                        "filter": "{}"
                    }
                ]
            }
        ]
    }
]

```

### Request

`GET http://example.com/rest/v1/syrinx/filter?storedFilter=true`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
storedFilter | N | "true:false" | Indicates whether the list should only contain stored filters.  If the parameter is not provided, then it will default to false.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.


