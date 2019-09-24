---
title: 7Park Data API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - python: Python

toc_footers:
  - <a href='https://account.7parkdata.com/#/akm_key/'>Sign Up for a Developer Key</a>
  - <a href='mailto:support@7parkdata.com'>Contact Us</a>
  - <a href='https://www.7parkdata.com/terms-of-use/'>Terms of Use</a>

includes:
  
search: false
---

# Introduction

At 7Park, we are on a mission to help organizations and people make better decisions by tapping into data. 

The Key Metrics API enables seamless access to 7Park Data’s portfolio of leading performance indicators, forecasts, and backtests.

For assistance with the Key Metrics API, please contact [support@7parkdata.com](mailto:support@7parkdata.com).

The 7Park Data API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

Click Run in Postman below to save every API request as a Postman collection for ease of testing.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/d89a9462e82593705d8a)

<!-- [![Open Swagger Spec](https://i.imgur.com/Y1oYTN0.png)](https://app.getpostman.com/run-collection/d89a9462e82593705d8a) -->

```shell
# BASE URL:
https://api.7parkdata.com
```

## Authentication

> To authenticate, query using your generated 'client_id' and 'client_secret':

```python
pip install -i https://pypi.7parkdata.com/simple/ parklib-sdk
# replace client_id / client_secret with your credentials.
export PARK_API_CLIENT_ID=<client_id>
export PARK_API_CLIENT_SECRET=<client_secret>
```

```shell
curl -X POST \
  https://api.7parkdata.com/oauth/token \
  -d '{
  "client_id": {client_id},
  "client_secret": {client_secret}
}'
# replace {client_id} and {client_secret} with your generated values from the developer portal
```

> The 'access_token' in the below example response will be your {token} for authentication for all future API Requests

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiI",
    "expires_in": 86400,
    "token_type": "Bearer"
}
```

7Park Data uses OAuth to authorize users to its APIs. You can register and generate client_id and client_secret credentials at our [developer portal](https://account.7parkdata.com/#/akm_key/)

To authenticate, in every API request, you must have the token in the header as such:

`Authorization: Bearer {token}`

You must replace <code>{token}</code> with the access token provided in the <code>/oauth/token</code> response.

<aside class="notice">
The access token expires every 24 hours. After which, you must get a new one to use to authenticate future API Requests.
</aside>

## Rate Limits

This API is rate limited.

The default user will receive the following usage plan:

* 10 requests per second
* burst 15 requests
* quota of 5,000 requests per week starting on Sunday

If you would like to inquire regarding increasing your rate limit, please contact [support@7parkdata.com](mailto:support@7parkdata.com).

## Permissions

Permissions are handled at a company level.

After you have been set up, you can verify which companies you have access to by querying this endpoint.

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/permissions"
headers = {"Authorization" : "Bearer " + access_token}
response = requests.request("GET", url, headers = headers, params = querystring)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/permissions' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "data_permission": [
    {
      "metric_id": 788,
      "metric_name": "Consumer Card Rev Index",
      "company_id": 14671,
      "company_name": "Papa John's Pizza"
    },
    {
      "metric_id": 738,
      "metric_name": "App Downloads (Android)",
      "company_id": 7097,
      "company_name": "Zillow"
    }
  ]
}
```



###### HTTP Request

`GET /permissions`

## Errors

The 7Park Data API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request
401 | Unauthorized
403 | Forbidden
404 | Not Found
429 | Too Many Requests
500 | Internal Server Error
502 | Bad Gateway
503 | Service Unavailable
504 | Gateway Timeout

# Metadata

The Metadata sections will help you build queries for retrieving Time Series and Forecast Data.

The Metadata required in all Time Series and Forecast queries are:

* `company_id`
* `metric_id`
* `entity_id`

## Company

### The Company Object

Parameter | Type | Description
--------- | ----------- | -----------
company_id | number | The company identifier.
company_name | string | The company name. E.g. `Netflix, Inc.`
ticker | string | Company ticker symbol. E.g. `NFLX`


### List all Companies

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/companies"
querystring = {"search" : "netflix"}
headers = {"Authorization" : "Bearer " + access_token}
response = requests.request("GET", url, headers = headers, params = querystring)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/companies/?search=netflix' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "company_id": 4354,
      "company_name": "Netflix, Inc.",
      "ticker": "NFLX"
    }
  ]
}
```

Find Available Companies.

###### HTTP Request

`GET /companies/`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
search | query string | false | string | Keyword search

### Retrieve a Company

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/4354"
headers = {"Authorization" : "Bearer " + access_token}
response = requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/4354' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "company_id": 4354,
  "company_name": "Netflix, Inc.",
  "ticker": "NFLX"
}
```

Get a Specific Company.

###### HTTP Request

`GET /company/{company_id}`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | The company identifier.

### Retrieve a Company's Hierarchy

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/4354/hierarchy"
headers = {"Authorization" : "Bearer " + access_token}
response = requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/4354/hierarchy' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "company_id": 4354,
    "parent_company_id": null,
    "ultimate_company_id": 4354,
    "subsidiaries": [
        {
            "company_id": 1399191,
            "external_name": "Netflix Pte Ltd.",
            "company_name": "Netflix Pte Ltd."
        },
        {
            "company_id": 1399190,
            "external_name": "NetflixCS, Inc.",
            "company_name": "NetflixCS, Inc."
        }
    ],
    "parent_company_name": null,
    "parent_company_ticker_region": null,
    "ultimate_parent_company_ticker_region": "NFLX-US",
    "ultimate_parent_company_name": "Netflix, Inc."
}
```

Retrieve Company Hierarchy.

###### HTTP Request

`GET /company/{company_id}/hierarchy`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | The company identifier.

## Metric

### The Metric Object

Parameter | Type | Description
--------- | ----------- | -----------
metric_id | number | The metric identifier.
metric_name | string | The metric name. E.g. `App Downloads`
metric_description | string | Description of the metric. E.g. `Count of Android & iOS app store downloads`


### List all Metrics

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/4354/metrics"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/4354/metrics' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "count": 9,
  "next": null,
  "previous": null,
  "results": [
    {
      "metric_id": 67,
      "metric_name": "App DAU Pcnt Panel",
      "metric_description": "Percent of app panel who are unique daily active users"
    },
    {
      "metric_id": 740,
      "metric_name": "App Downloads",
      "metric_description": "Count of Android & iOS app store downloads"
    }
  ]
}
```

List all Company Metrics.

###### HTTP Request

`GET /company/{company_id}/metrics`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | company_id

### Retrieve a Metric

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/4354/metric/740"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/4354/metric/740' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "metric_id": 740,
    "metric_name": "App Downloads",
    "metric_description": "Count of Android & iOS app store downloads"
}
```

Retrieve a Company metric.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | company_id
metric_id | path | true | number | metric_id

## Entity

### The Entity Object

Parameter | Type | Description
--------- | ----------- | -----------
entity_id | number | The entity identifier.
entity_name | string | The entity name. E.g. `Uber Eats: Local Food Delivery`

### List all Entities 

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/14301/metric/740/entities"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/14301/metric/740/entities' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "count": 4,
  "next": null,
  "previous": null,
  "results": [
    {
      "entity_id": 1931919,
      "entity_name": "Uber & Uber Eats"
    },
    {
      "entity_id": 1927747,
      "entity_name": "Uber"
    },
    {
      "entity_id": 1927749,
      "entity_name": "Uber Eats: Local Food Delivery"
    },
    {
      "entity_id": 1927748,
      "entity_name": "Uber Driver"
    }
  ]
}
```

Get Entities.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entities`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | company_id
metric_id | path | true | number | metric_id

### Retrieve a Entity 

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/14301/metric/740/entity/1927749"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/14301/metric/740/entity/1927749' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "entity_id": 1927749,
    "entity_name": "Uber Eats: Local Food Delivery"
}
```

Get Entities.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entity/{entity_id}`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | number | company_id
metric_id | path | true | number | metric_id



# Time Series

## Retrieve Metadata

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/14301/metric/740/entity/1927747/queries"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/14301/metric/740/entity/1927747/queries' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "queries": [
    {
      "entity_id": 1927747,
      "metric_id": 740,
      "metric_periodicity": "Daily",
      "discovery_name": "Uber",
      "country_name": "Mexico",
      "company_name": "Uber"
    },
    {
      "entity_id": 1927747,
      "metric_id": 740,
      "metric_periodicity": "Daily",
      "discovery_name": "Uber",
      "country_name": "Brazil",
      "company_name": "Uber"
    }
  ]
}
```

Retrieve Metadata.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entity/{entity_id}/queries`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | string | company_id
metric_id | path | true | string | metric_id
entity_id | path | true | string | entity_id


## Retrieve Timeseries

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/data?entity_id=1927747&metric_periodicity=Daily&country_name=United%20States%20of%20America&metric_id=740"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/data?entity_id=1927747&metric_periodicity=Daily&country_name=United%20States%20of%20America&metric_id=740' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "status": "ok",
    "query": {
        "msa_name": "N/A",
        "entity_id": 1927747,
        "metric_id": 740,
        "metric_periodicity": "Daily",
        "date_from": "2019-01-01",
        "world_region_name": "N/A",
        "us_region_name": "N/A",
        "us_state_name": "N/A",
        "date_to": "2019-08-01",
        "country_name": "United States of America",
        "cohort_descripton": "{\"gender\":\"N/A\",\"age_group\":\"N/A\",\"cohort_grouping\":\"N/A\"}"
    },
    "errors": [],
    "data": [
        {
            "world_region_name": "N/A",
            "metric_id": 740,
            "company": "Uber",
            "us_region_name": "N/A",
            "value": 83165.0,
            "date": "2019-01-01",
            "period_end": "2019-01-01",
            "country_name": "United States of America",
            "metric_name": "App Downloads"
        },
        {
            "world_region_name": "N/A",
            "metric_id": 740,
            "company": "Uber",
            "us_region_name": "N/A",
            "value": 70827.0,
            "date": "2019-01-02",
            "period_end": "2019-01-02",
            "country_name": "United States of America",
            "metric_name": "App Downloads"
        }
    ]
}
```

Data: Retrieve a time series for a specific entity/metric

###### HTTP Request

`GET /data`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
metric_id | path | true | string | metric_id
entity_id | path | true | string | entity_id
metric_periodicity | path | true | string | metric_periodicity
country_name | path | false | string | country_name
date_from | path | false | string | date_from
date_to | path | false | string | entity_id
world_region_name | path | false | string | world_region_name
us_region_name | path | false | string | us_region_name
discovery_name | path | false | string | discovery_name

# Forecasts

## List all Forecasts

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/forecasts"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/forecasts' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
  "count": 124,
  "next": "https://api.7parkdata.com/forecasts/?page=2",
  "previous": null,
  "results": [
    {
      "company_id": 2010,
      "company_name": "Facebook",
      "entity_id": 1931918,
      "entity_name": "Facebook & Instagram",
      "metric_id": 740,
      "metric_name": "App Downloads",
      "forecast_metric_name": "Total MAU",
      "recent_data_through": "2019-06-24"
    },
    {
      "company_id": 10141,
      "company_name": "Lululemon Athletica",
      "entity_id": 56810,
      "entity_name": "lululemon",
      "metric_id": 761,
      "metric_name": "Receipt Revenue Index",
      "forecast_metric_name": "Adjusted Revenue",
      "recent_data_through": "2019-08-10"
    }
  ]
}
```

List all Forecasts.

###### HTTP Request

`GET /forecasts/`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
search | query string | false | string | Keyword search
company_id | query string | false | string | company_id
ordering | query string | false | string | ordering
page | query string | false | string | page
page_size | query string | false | string | page_size


## Forecast Snapshots

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/14301/metric/740/entity/1940111/forecast/snapshot"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/14301/metric/740/entity/1940111/forecast/snapshot' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "business_entity_name": "Netflix",
    "metricvals": [
        {
            "forecast_model": "Y/Y",
            "period_effective_end_date": "2017-06-30",
            "methodology": "unspecified",
            "forecast": 47787.1869370347,
            "data_through": "2018-07-09",
            "period_effective_start_date": "2017-04-01",
            "is_published": "N"
        },
        {
            "forecast_model": "Y/Y",
            "period_effective_end_date": "2017-09-30",
            "methodology": "unspecified",
            "forecast": 52153.251969142,
            "data_through": "2018-07-09",
            "period_effective_start_date": "2017-07-01",
            "is_published": "N"
        },
        {
            "forecast_model": "Y/Y",
            "period_effective_end_date": "2017-12-31",
            "methodology": "unspecified",
            "forecast": 54009.2938156461,
            "data_through": "2018-07-09",
            "period_effective_start_date": "2017-10-01",
            "is_published": "N"
        },
    ],
    "metric_id": 768,
    "company_id": 4354,
    "forecast_metric": "U.S. Subscribers",
    "company_name": "Netflix, Inc.",
    "lpi": "App Downloads"
}
```

Snapshot of forecasts as published in Excel reports.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entity/{entity_id}/forecast/snapshot`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | string | company_id
metric_id | path | true | string | metric_id
entity_id | path | true | string | entity_id


## Forecast Data

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/4354/metric/740/entity/1940111/forecast"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/4354/metric/740/entity/1940111/forecast' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "forecast_metric_name": "U.S. Subscribers",
    "forecast_unit": "",
    "period_effective_start_date": "2019-04-01",
    "period_effective_end_date": "2019-06-30",
    "entity_name": "Netflix",
    "permission": true,
    "data": [
        {
            "forecast": 60588.4693430389,
            "r_squared": "",
            "correlation": "",
            "description": "Based on quarter over quarter quarterly growth rates.",
            "queries": [
                {
                    "entity_id": 1916255,
                    "metric_id": 768,
                    "metric_periodicity": "Quarter_Over_Quarter",
                    "metric_type": "7Park Data"
                },
                {
                    "entity_id": 1916255,
                    "metric_id": 704,
                    "metric_periodicity": "Quarter_Over_Quarter",
                    "metric_type": "reported"
                }
            ]
        },
        {
            "forecast": 64069.7707137404,
            "r_squared": "",
            "correlation": "",
            "description": "Based on year over year quarterly growth rates.",
            "queries": [
                {
                    "entity_id": 1916255,
                    "metric_id": 768,
                    "metric_periodicity": "Year_Over_Year",
                    "metric_type": "7Park Data"
                },
                {
                    "entity_id": 1916255,
                    "metric_id": 704,
                    "metric_periodicity": "Year_Over_Year",
                    "metric_type": "reported"
                }
            ]
        }
    ]
}
```

Forecast info for company, metric and entity.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entity/{entity_id}/forecast`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | string | company_id
metric_id | path | true | string | metric_id
entity_id | path | true | string | entity_id


## Historical Forecasts

> Example Request:

```python
import requests

url = "https://api.7parkdata.com/company/14301/metric/740/entity/1940111/forecast/history"
headers={"Authorization" : "Bearer " + access_token}
response=requests.request("GET", url, headers = headers)
print(response.text)
```

```shell
curl -X GET \
  'https://api.7parkdata.com/company/14301/metric/740/entity/1940111/forecast/history' \
  -H 'Authorization: Bearer {token}' \
```

> Example Response:

```json
{
    "status": "ok",
    "query": {
        "reported_metric_id": 704,
        "entity_id": 1916255,
        "metric_id": 768
    },
    "errors": [],
    "data": [
        {
            "metric_id": 768,
            "metric_periodicity": "Quarter_Over_Quarter",
            "entity_name": "Netflix",
            "company": "Netflix, Inc.",
            "period_effective_end_date": "2018-03-31",
            "methodology": "unspecified",
            "forecast": 56060.01019,
            "data_through": "2018-04-09",
            "forecast_unit": null,
            "correlation": null,
            "period_effective_start_date": "2018-01-01",
            "r_squared": null,
            "metric_name_reported": "U.S. Subscribers",
            "metric_name": "DownloadsAndroidiOS"
        }
    ],
    "description": "company name: Netflix, Inc.; entity: Netflix; dimensions: {}; metric: DownloadsAndroidiOS; metric_periodicity: N/A"
}
```

Historical forecast info for company, metric and entity.

###### HTTP Request

`GET /company/{company_id}/metric/{metric_id}/entity/{entity_id}/forecast/history`

###### Parameters

Parameter | In | Required | Type | Description
--------- | -- | -------- | ----------- | -----------
company_id | path | true | string | company_id
metric_id | path | true | string | metric_id
entity_id | path | true | string | entity_id


# Platform

## Overview

Cleaning real world data is a painstaking process. Make it easier by using the same tools that 7Park Data uses to deliver insights to hundreds of the leading global financial services companies.
7Park Data’s Python SDK has the core functionality you need to prepare analytics ready data.

Derive the most value from your data with powerful technology proven to accelerate data initiatives, by increasing capacity for internal engineering and data science teams while reducing costs and operational risk. Our machine learning-powered models outperform comparable solutions with a 4x higher F1 score. Designed for scale and versatility, our Platform processes over 4 TB of transaction and natural language data daily.

7Park Data’s tagging, cleansing and linking solutions can be used as part of a comprehensive data preparation program, or for more targeted projects.

Our solutions transform raw data into analytics-ready information, at scale. Here is how our 3 core modules can help:

* `7Park Tag` (Named Entity Recognition): Bring structure and meaning to make unstructured data useful
* `7Park Cleanse` (Named Entity Disambiguation): Reduce noise and dimensionality by resolving entities in data
* `7Park Link` (Named Entity Linking): Connect and enrich by connecting multiple data sets

If you have technical questions please contact Joel at [joel@7parkdata.com](mailto:joel@7parkdata.com).

For product feedback and ideas please reach out to Malte at [malte@7parkdata.com](mailto:malte@7parkdata.com).

## Quick Start Guide

1) Set up your credentials and request access.

  * Visit the [Account Management Page](http://account.7parkdata.com/#/akm_key/)
  * Click `Request Credentials` in the Authentication section.
  * Copy the `client_id` & `client_secret`.
  * Contact customer success to enable 7Park Platform Modules: [support@7parkdata.com](mailto:support@7parkdata.com)

<br/>

2) Install the 7Park Platform SDK from 7Park Data’s pypi server and set your credentials.

<div class="center-column"></div>
```python
pip install -i https://pypi.7parkdata.com/simple/ parklib-sdk
# replace client_id / client_secret with your credentials.
export PARK_API_CLIENT_ID=<client_id>
export PARK_API_CLIENT_SECRET=<client_secret>
```

<br/>

3) Set s3 Bucket Policy (only required for Bulk functions)

* Create private s3 bucket
* Navigate to your s3 bucket > permissions > bucket policy
* Paste in the following json (replace `{BUCKET_NAME}` with your bucket's name):

<div class="center-column"></div>
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::084888172679:role/platform-services"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::{BUCKET_NAME}/*"
        }
    ]
}
```

<br/>


4) Start coding.

<div class="center-column"></div>
```python
from parklib.sdk.ner import NER
ner_result = NER.execute(
    source_type='news',
    instances=["Netflix beats the street.","Amazon expands to Austin."]
)
print(ner_result)
```

## Examples

Explore common use cases that leverage 7Park Platform SDK to Tag, Link, or Cleanse data.

### Enrich records in a SQL table with their parent company’s ticker symbol

1) Load your local SQL table into a dataframe using pyodbc or any python db client you prefer

<div class="center-column"></div>
```python
import pyodbc
import pandas as pd
conn_str = (
    "DRIVER={PostgreSQL Unicode};"
    "DATABASE=<database>;"
    "UID=postgres;"
    "PWD=<password>;"
    "SERVER=<server>;"
    "PORT=5432;"
    )

conn = pyodbc.connect(conn_str)

SQL_Query = pd.read_sql_query(
'''select
company_name
from my_company_database''', conn)

df = pd.DataFrame(SQL_Query,
columns=['name'])
```

<br/>

2) Use Parklib SDK to enrich each row of the dataframe with its parent company’s ticker symbol

<div class="center-column"></div>
```python
from parklib.sdk.nel import NEL
from parklib.sdk.hierarchy import Hierarchy
df['parent_ticker'] = None
def append_ticker(item):
    resp = NEL.execute(company_name=item['name'])
     links = ['message']['links']
     if links: # We found a potential link
        company_id = links[0]['company_id']
        resp = Hierarchy.get_parent_info(company_id)
        item['parent_ticker'] = resp['parent_ticker'] # Add parent_ticker
    return item
df.apply(append_ticker,axis=1)
```


### Disambiguate a column of a sql table and link to a canonical company

1) Load your local SQL table into a dataframe using pyodbc or any python db client you prefer

<div class="center-column"></div>
```python
import pyodbc
import pandas as pd
conn_str = (
    "DRIVER={PostgreSQL Unicode};"
    "DATABASE=<database>;"
    "UID=postgres;"
    "PWD=<password>;"
    "SERVER=<server>;"
    "PORT=5432;"
    )

conn = pyodbc.connect(conn_str)

SQL_Query = pd.read_sql_query(
'''select
company_name
from my_company_database''', conn)

df = pd.DataFrame(SQL_Query,
columns=['company_names'])
```

<br/>

2) Upload to AWS S3

<div class="center-column"></div>
```python
import boto3

df.to_csv("data.tsv", sep="\t", index=False)  # temp file can be removed after upload to S3
s3.Object(bucket_name, 'path/to/file.tsv').put(Body=open('data.tsv', 'rb'))
```
<br/>

2) Use 7Park NED engine to disambiguate the data

<div class="center-column"></div>
```python
from parklib.sdk.ned import NED

job_id = NED.run_job_transform(target_col_index=0,
                       input_path="s3://bucket_name/path/to/file.tsv",
                       output_path="s3://bucket_name/path/for/output")
print(job_id)
{'bulk_id': 'bulk-ned-349f81b4-2fb5-451f-825c-65243610ebfc'}
```

<div class="center-column"></div>
```python

```


## 7Park Tag

7Park Tag (Named Entity Recognition)

Create structured datasets from raw inputs

Parse and tag raw, unstructured data to add value to your analytical strategies, and avoid losing precious time trying to make sense of messy files. 7Park Data’s Tag solution brings structure and meaning to your data by identifying and classifying entities in natural language and transaction data, including merchant names, organizations, locations, brands, products, people and more.

### Execute

Run Name entity recognition in real time.

> Example Request:

```python
from parklib.sdk.ner import NER

ner_result = NER.execute(source_type='news', instances=["Apple is a company"])
print(ner_result)
```

```shell
curl -X POST \
  https://api.7parkdata.com/ner/annotate \
  -H 'Authorization: Bearer {token}' \
  -d '{
       "source_type": "news",
       "instances": [
              "Apple is a company."
       ]
}'
```

> Example Response:

```json
{
    "results": [
        {
            "doc_index": 0,
            "preprocessed": "Apple is a company.",
            "ner": [
                {
                    "start_pos": 0,
                    "type": "NE_ORG",
                    "key": "Apple",
                    "end_pos": 5
                }
            ]
        }
    ]
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
source_type | true | string | Source type to consider. Options include: `news`, `tweets`, and `credit card`
entity_types | false | array | Entity types to consider. Options include: `NE_TITLE`, `NE_PERSON`, `NE_ORG`, `NE_PRODUCT`, `NE_GPE`, `NE_CURRENCY`, `NE_MERCHANT`, `NE_STORE`, `NE_PAYMENT`, and `NE_DATE`.
instances | true | array | A list of strings that you wish to be recognized.

### Bulk

Run NER bulk job transform.

> Example Request:

```python
from parklib.sdk.ner import NER

job_id = NER.run_job_transform(source_type="news",
                               entity_types=["NE_TITLE", "NE_PERSON"],
                               input_path="s3://7parkdata/example/example.txt",
                               output_path="s3://7parkdata/example/output")
print(job_id)
```

```shell

```

> Example Response:

```json
{
  "bulk_id": "bulk-ner-349f81b4-2fb5-451f-825c-65243610ebfa"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
source_type | true | string | Source type to consider. Options include: `news`, `tweets`, and `credit card`
entity_types | false | array | Entity types to consider. Options include: `NE_TITLE`, `NE_PERSON`, and `NE_ORG`.
input_path | true | string | S3 file path. Example: `s3://7p-sdk-examples/bulk_ner_example.jsonl` The file should be a [jsonlines](http://jsonlines.org/) text file format
output_path | true | string | S3 folder path. Example: `s3://7p-sdk-examples/output`


### Status

Check the status of NER job.

> Example Request:

```python
from parklib.sdk.ner import NER

job_status = NER.job_transform_status('bulk-ner-349f81b4-2fb5-451f-825c-65243610ebfb')
print(job_status)
```

```shell

```

> Example Response:

```json
{
  "error": None,
  "output_path": None,
  "status": "InProgress"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
job_id | true | string | The job id return from run_job_transform






## 7Park Cleanse

7Park Cleanse (Named Entity Disambiguation)

Reduce noise and create a standard lexicon

Gain clarity into your data by disambiguating text to establish a single semantic reference for better analysis and enrichment. Our Cleanse solution efficiently and accurately creates consistent text from natural language or transaction data, expediting work that would otherwise be sidelined by discrepancies in spelling, entry methods and miscategorization.

### String Distance

Run NED - String Distance Model.

> Example Request:

```python
from parklib.sdk.ned import StringDistanceNED

job_id = StringDistanceNED.run_job_transform(target_col_index=3,
                               input_path="s3://7parkdata/example/example.txt",
                               output_path="s3://7parkdata/example/output",
                               threshold=0.009,
                               stopwords=["company", "co", "corp", "llc"])
print(job_id)
```

```shell

```

> Example Response:

```json
{
  "bulk_id": "bulk-ned-349f81b4-2fb5-451f-825c-65243610ebfc"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
target_col_index | true | integer | Target column index. Start from `0`. Example: `3`
input_path | true | string | S3 path for TSV format file. Example: `s3://7parkdata/example/example.tsv`
output_path | true | string | S3 folder path. Example: `s3://7parkdata/example/output`
threshold | true | float | Threshold for ned model. default: `0.01` Example: `0.009`
stopwords | true | list | Stopwords. Example: `[“company”, “co”, “corp”, “llc”]`

### Word Embedding

Run NED - Word Embedding Model.

> Example Request:

```python
from parklib.sdk.ned import WordEmbeddingNED

job_id = WordEmbeddingNED.run_job_transform(target_col_index=3,
                                            input_path="s3://7parkdata/example/example.txt",
                                            output_path="s3://7parkdata/example/output",
                                            supplemental_col_indexes=[2, 3, 4],
                                            angular_distance_threshold=0.4,
                                            max_variation_number=500,
                                            stopwords=["company", "co", "corp", "llc"])
print(job_id)
```

```shell

```

> Example Response:

```json
{
  "bulk_id": "bulk-ned-349f81b4-2fb5-451f-825c-65243610ebfc"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
target_col_index | true | integer | Target column index. Start from `0`. Example: `3`
input_path | true | string | S3 path for TSV format file. Example: `s3://7parkdata/example/example.tsv`
output_path | true | string | S3 folder path. Example: `s3://7parkdata/example/output`
supplemental_col_indexes | true | list | A list of supplemental column index
angular_distance_threshold | true | float | Threshold for word embedding ned model. Default: 0.4 Example: `0.5`
max_variation_number | true | float | Threshold for word embedding ned model. Default: 500. Example: `500`
stopwords | true | list | Stopwords. Example: `[“company”, “co”, “corp”, “llc”]`

### Status

Check the status of NED job transform.

> Example Request:

```python
from parklib.sdk.ned import NED

job_status = NED.job_transform_status('bulk-ned-349f81b4-2fb5-451f-825c-65243610ebfc')
print(job_status)
```

```shell

```

> Example Response:

```json
{
    "status": "RUNNING",
    "output_path": null,
    "error": null
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
job_id | true | string | The job id return from run_job_transform



## 7Park Link

7Park Link (Named Entity Linking)

Add context by connecting multiple datasets

Expand the utility of your datasets by seamlessly adding descriptive and contextual information from multiple reference sources. 7Park Data’s Link solution creates robust, valuable datasets by joining alternative and fundamental data sets, connecting related entities, organizing by corporate hierarchy and mapping to ticker.

### Execute

Run Name entity Linking on real time.

> Example Request:

```python
from parklib.sdk.nel import NEL

nel_result = NEL.execute(company_name="7park")
print(nel_result)
```

```shell

```

> Example Response:

```json
{
  "message": {
    "Confidence Result in Set": 0.0,
    "Top link confidence": 0.9,
    "links": [
      {
        "company_id": 378567,
        "confidence": 0.9,
        "factset_id": "0FM8C2-E",
        "name": "7Park Data, Inc.",
        "ticker_region": None,
        "website": "http://www.7parkdata.com"
      },
      {
        "company_id": 1479086,
        "confidence": 0.06,
        "factset_id": "0C3LPN-E",
        "name": "Park Vida Group, Inc.",
        "ticker_region": "PRKV-US",
        "website": "http://www.parkvida.com"
      }
    ],
    "search_args": {
      "company_name": "7park",
      "ticker": "",
      "website": ""
    },
    "top_name": "7Park Data, Inc.",
    "top_website": "https://www.7parkdata.com/"
  }
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
ticker | true | string | Ticker
company_name | true | string | Company name
website | true | string | Website
brand_name | true | string | Brand name
industries | true | list | Industries
sectors | true | list | Sectors

### Bulk

Run NEL bulk job transform.

> Example Request:

```python
from parklib.sdk.nel import NEL

job = NEL.run_job_transform(input_path="s3://7parkdata/example/example.txt",
                            output_path="s3://7parkdata/example/output")
print(job)
```

```shell

```

> Example Response:

```json
{
  "bulk_id": "bulk-nel-349f81b4-2fb5-451f-825c-65243610ebfa"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
input_path | true | string | S3 file path. Example: `s3://7parkdata/example/example.txt` The file should be a [jsonlines](http://jsonlines.org/) text file format
output_path | true | string | S3 folder path. Example: `s3://7parkdata/example/output`



### Status

Check the status of NEL job.

> Example Request:

```python
from parklib.sdk.nel import NEL

job_status = NEL.job_transform_status('bulk-nel-349f81b4-2fb5-451f-825c-65243610ebfb')
print(job_status)
```

```shell

```

> Example Response:

```json
{
  "error": None,
  "output_path": None,
  "status": "InProgress"
}
```

###### Parameters

Parameter | Required | Type | Description
--------- | -------- | ----------- | -----------
job_id | true | string | bulk_id return from run_job_transform