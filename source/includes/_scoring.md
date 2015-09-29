# Scoring

## Get List of Scoring Algorithms
```shell
# Get List of Scoring Algorithms

curl -X "GET" "http://example.com/rest/v1/syrinx/scorings" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"

# JSON Result

{
    "data": "{ID1=framingham, ID2=dpp, ID3=AUSframingham}",
    "message": "OK",
    "status": 200
}
```

### Request

`GET http://example.com/rest/v1/syrinx/scorings`

### Parameters
none

### Return
If the request succeeded, then HTTP 200 is returned along with OK message.

## Score Data

```shell
# Score Data

curl -X "POST" "http://example.com/rest/v1/syrinx/scoring" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789" \
	-d $'{   
    "scoringID"         : "ID1", 
    "saved_filter_id"   : "db60216d-d186-4493-ac4d-442ef2355714",
    "callback_url"      : ""
}'

# JSON Result

{
    "jobId": "JOB1443454034168",
    "message": "Accepted",
    "status": 202
}
```

### Request

`POST http://example.com/rest/v1/syrinx/scoring`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
scoringID | Y | "" | Indicates the scoring algorithm to use
saved_filter_id | Y | "" | Indicates a previously saved filter id.
callback_url | N | "http://example.com/url" | Used to call back an endpoint with the status of the request

### Return
If the request succeeded, then HTTP 202 is returned along with a job id, meaning that the job was accepted for processing.

## Status of Scoring

```shell
curl -X "GET" "example.com/rest/v1/syrinx/scoring/status/JOB1443454692764" \
	-H "Content-Type: application/json" \
	-H "clientToken: 123456789"
```

### Request

`GET http://example.com/rest/v1/syrinx/scoring/status/{jobId}`

### Parameters

Parameter | Req | Value | Description
--------- | ------- | ------ | -----------
{jobId} | Y | "" | The job id of a previously submitted job.

### Return
If the request succeeded, then HTTP 202 is returned along with COMPLETE message.

