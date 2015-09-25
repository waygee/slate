# Aggregate
The aggregate endpoint is used to aggregate data over specified parameters in the patient data store.  For Syrinx 1.0, this is a wrapper API for MongoDB’s aggregation capabilities. An aggregation pipeline consists of three steps, a projection that specifies which fields to project out from the data, (the first part of the group stage of the mongodb aggregation pipeline) an aggregation stage that specifies the parameters to aggregate over along with associated commands (the accumulation part of the group stage of the MongoDB pipeline), and the filter stage which specifies any additional filter on the data you would like to impose. (This is the match stage of the MongoDB aggregation pipeline)

To create an aggregate and get the data requires four api calls

- Create Aggregate
- Get Status of Aggregate
- Retrieve Data by Aggregate ID
- Get Status of Retrieve Operation

## Create Aggregate

```shell
# Create Aggregate

curl -X "POST" "http://example.com/rest/v1/syrinx/aggregates" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789" \
	-d $'{        
    "pattern": "groupBy",
    "projection": "data.demographics.ethnicity",
    "aggregate": "count:$sum",
    "filter": "{}",
    "callback_url": ""
}'

# JSON Result

{
    "jobId": "JOB1443207422508",
    "message": "Accepted",
    "status": 202
}

```

### Request

`POST http://example.com/rest/v1/syrinx/aggregates`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
pattern | Y | "groupBy" | Only the group by pattern is supported at this time. 
projection | Y | "" | Limits the fields to return for all matching documents.  Projection is specified as a comma separated list of fields. 
aggregate | Y | "" | Parameters and aggregation commands (i.e. average, sum, count, etc) to run the aggregation process on the data.
filter | Y | "{}", "[]", "filterId" | Can be a JSON object, array, or string.  JSON Objects conform to MongoDB query rules. If an array is passed, the API would expect either a JSON object or a string as part of the array. If the array element is a string then it is treated as filterId whereas JSON Object is treated as filter condition.  Please see specific examples of filter parameter below.
target | N | "standard:scored:filtered" | Where to run aggregates. Valid options are "standard", "scored", "filtered"
title | N | "" | An optional title
subtitle | N | "" | An optional subtitle
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the job was accepted for processing.

### Example Usages of Filter Parameter
*Example 1*

filter = {"data.demographics.gender":"Male"}

*Example 2*

filter = [{"data.demographics.gender":"Male"}, {"data.demographics.race":"White"}]

*Example 3*

filter = ["a0c6678b-b8ec-4e1e-befe-a9f05fa91d83", "4feabc09-01e7-4762-9443-a59d7b1bf4d3"] // List of filterIds

*Example 4*

filter = [{"data.demographics.gender":"Male"}, "4feabc09-01e7-4762-9443-a59d7b1bf4d3", {"data.demographics.race":"White"}, "FilterId"]



## Get Status of Aggregate
```shell
# Get Status of Aggregate

curl -X "GET" "http://example.com/rest/v1/syrinx/aggregates/status/JOB1443207422508" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"

# JSON Result

[
    {
        "message": "COMPLETE",
        "status": 200,
        "data": [
            {
                "aggregateId": "f184078e-ff7b-4954-aa49-25f8954fedec"
            }
        ]
    }
]
```


### Request

`GET http://example.com/rest/v1/syrinx/aggregates/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | Y | "" | The job id of a previously submitted job.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.

<aside class="notice">Please note, an aggregateId is returned in this api call.  You will use aggregateID in the next api call.</aside>


## Retrieve Data by Aggregate ID

```shell
# Retrieve Data by Aggregate ID

curl -X "GET" "http://example.com/rest/v1/syrinx/aggregates/f184078e-ff7b-4954-aa49-25f8954fedec" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"

# JSON Result

{
    "jobId": "JOB1443208893613",
    "message": "Accepted",
    "status": 202
}
```

### Request

`GET http://example.com/rest/v1/syrinx/aggregates/{aggregateId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{aggregateId} | Y | "" | The aggregate id of a previously submitted job.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.


## Get Status of Retrieve Operation

```shell
# Get Status of Retrieve Operation

curl -X "GET" "http://example.com/rest/v1/syrinx/aggregates/status/JOB1443208893613" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"

# JSON Result

[
    {
        "message": "COMPLETE",
        "status": 200,
        "data": [
            {
                "_id": {
                    "$oid": "560598fecd7f89661b43bbc0"
                },
                "aggregate_data": {
                    "category": [
                        {
                            "name": "Not Hispanic or Latino",
                            "data": 1
                        }
                    ]
                },
                "aggregate_id": "f184078e-ff7b-4954-aa49-25f8954fedec",
                "meta": [
                    {
                        "aggregate": "count:$sum",
                        "projection": "data.demographics.ethnicity",
                        "target": "standard",
                        "filter": "{}"
                    }
                ]
            }
        ]
    }
]
```

### Request

`GET http://example.com/rest/v1/syrinx/aggregates/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | Y | "" | The job id of a previously submitted job.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.

<aside class="success">The data is returned in this operation.</aside>

## Get List of Aggregates

### Request

`GET http://example.com/rest/v1/syrinx/aggregates`

### Parameters
none

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.
