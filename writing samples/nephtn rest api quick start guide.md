<details>
<summary><i>Background</i></summary>

I used the NEPHTN API for my capstone project in college. I was amazed at the API's power, but overwhelmed by the 100+ page user guide which was the API's only public documentation. It took days of confusion before my team and I realized that 90% of this documentation was mostly irrelevant, since you could use the API for its main purpose with just a few endpoints.

I wrote this quick start guide to provide developers with a clear, concise, easily-scannable reference for the API's main functionality. The intended audience for this document is the software developer who is familiar with REST APIs, but has no experience using the NEPHTN API.

</details>
</br>

[//]: # (markdown structure inspired by https://gist.github.com/azagniotov/a4b16faf0febd12efbc6c3d7370383a6)
[//]: # (This document is a condensed rewrite of the NEPHTN API's existing documentation, which the CDC owns. I copied some snippets from the API's existing documentation, such as response schemas for a few endpoints. However, the vast majority of this document is my own writing.)

# NEPHTN REST API Quick Start Guide

## Contents:
- [Overview](#overview)
- [Authentication](#authentication)
- [Endpoints](#endpoints)

## Overview <a name="overview"></a>

The National Environmental Public Health Tracking Network (NEPHTN) API is a REST API maintained by the CDC. The NEPHTN API gives developers access to various American public health data.

This document is an abbreviated API guide showing only the most essential features of the NEPHTN API. For full documentation, refer to [https://ephtracking.cdc.gov/apihelp](https://ephtracking.cdc.gov/apihelp).

Data in the NEPHTN API is organized as follows:
- A <b>Content Area</b> is a broad category of public health metrics, e.g. 'Cancer' or 'Birth Defects'.
- An <b>Indicator</b> is a more specific category of public health metrics within a Content Area, e.g. 'Bladder Cancer' or 'Lung Cancers'.
- A <b>Measure</b> is an individual public health metric within an Indicator, e.g. 'Age-adjusted Incidence Rate of Lung and Bronchus Cancer per 100,000 Population' or 'Annual Number of Cases of Lung and Bronchus Cancer'.
- Each Measure's data is organized by location and time period.
- You can sort each Measure's data across different populations, called <b>Stratification Levels</b>.

To view data for a public health metric, you will call the <code>GetCoreHolder</code> endpoint using the following inputs:
- a measure
- geographic information
- temporal information
- a stratification level

## Authentication <a name="authentication"></a>

For complete access to the API, obtain an API token by contacting nephtrackingsupport@cdc.gov. Without an API token, you will still have limited access to the API. This guide will only show examples which you can access without a token.

## Endpoints <a name="endpoints"></a>
- All endpoints use the base URL ```ephtracking.cdc.gov/apigateway/api/v1```.
- All responses will be in JSON.

#### Content Areas <a name="contentareas"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>contentareas/json</code> | List all content areas</summary>

##### Path parameters

> none

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
> {
>   "type": "array",
>   "items": {
>     "type": "object",
>     "properties": {
>       "ContentAreaId": {
>         "type": "string"
>       },
>       "ContentAreaName": {
>         "type": "string"
>       },
>       "ContentAreaShortName": {
>         "type": "string"
>       }
>     }
>   }
> }
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/contentareas/json?apiToken=YOURAPITOKEN"
> ```

##### Example response

> ```javascript
> [
>   {"id":"11","name":"Air Quality","shortName":"AQ"},
>   {"id":"3","name":"Asthma","shortName":"AS"},
>   {"id":"9","name":"Cancer","shortName":"CR"},
>   {"id":"2","name":"Carbon Monoxide Poisoning","shortName":"CO"},
>   {"id":"10","name":"Childhood Cancers","shortName":"CHCR"},
>   {...}
> ]
> ```

</details>

------------------------------------------------------------------------------------------

#### Indicators <a name="indicators"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>indicators/{ContentAreaId}</code> | List indicators for a content area</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | ContentAreaId | required | int | Unique numeric identifier for your desired content area. Obtain using the [Content Areas](#contentareas) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "IndicatorId": {
>        "type": "string"
>      },
>      "IndicatorName": {
>        "type": "string"
>      },
>      "IndicatorShortName": {
>        "type": "string"
>      },
>      "externalURL": {
>        "type": "null"
>      },
>      "externalURLText": {
>        "type": "null"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/indicators/9?apiToken=YOURAPITOKEN"
> // ContentAreaId 9 corresponds to 'Cancer'
> ```

##### Example response

> ```javascript
>[
>  {
>     "id": "21",
>     "name": "Incidence of Acute Myeloid Leukemia",
>     "shortName": "Myeloid Leuk",
>     "externalURL": null,
>     "externalURLText": null
>  },
>  {
>     "id": "18",
>     "name": "Incidence of Bladder Cancer",
>     "shortName": "Bladder Cancer",
>     "externalURL": null,
>     "externalURLText": null
>  },
>  {
>     "id": "47",
>     "name": "Incidence of Brain & Other Nervous System Cancer",
>     "shortName": "Brain & Other ",
>     "externalURL": null,
>     "externalURLText": null
>  },
>  ...
>]
> ```

</details>

------------------------------------------------------------------------------------------

#### Measures <a name="measures"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>measures/{IndicatorId}</code> | List measures for an indicator</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | IndicatorId | required | int | Unique numeric identifier for your desired indicator. Obtain using the [Indicators](#indicators) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "MeasureId": {
>        "type": "string"
>      },
>      "MeasureName": {
>        "type": "string"
>      },
>      "MeasureShortName": {
>        "type": "string"
>      },
>      "externalURLText": {
>       "type": "null"
>      },
>      "externalURLText": {
>        "type": "null"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/measures/25?apiToken=YOURAPITOKEN"
> // IndicatorId 25 corresponds to 'Incidence of Thyroid Cancer'
> ```

##### Example response

> ```javascript
>[
>  {
>    "id": "66",
>    "name": "Age-adjusted Incidence Rate of Thyroid Cancer per 100,000 Population",
>    "shortName": "Age-adjusted rate per 100,000 population",
>    "externalURL": null,
>    "externalURLText": null
>  },
>  {
>    "id": "226",
>    "name": "Age-adjusted Incidence Rate of Thyroid Cancer per 100,000 Population over a 5-year Period",
>    "shortName": "Age-adjusted rate per 100,000 population over a 5 year period",
>    "externalURL": null,
>    "externalURLText": null
>  },
>  {
>    "id": "65",
>    "name": "Annual Number of Cases of Thyroid Cancer",
>    "shortName": "Annual number of cases",
>    "externalURL": null,
>    "externalURLText": null
>  },
>  ...
>]
> ```

</details>

------------------------------------------------------------------------------------------

#### Geographic Information <a name="geographic"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>geographictypes/{MeasureID}</code> | List available geographic levels for a measure</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "id": {
>        "type": "integer"
>      },
>      "GeographicTypeId": {
>        "type": "integer"
>      },
>      "GeographicType": {
>        "type": "string"
>      },
>      "SelectOptionsTypeId": {
>        "type": "integer"
>      },
>      "SelectOptionsType": {
>        "type": "string"
>      },
>      "SmoothingLevelId": {
>        "type": "integer"
>      },
>      "Smoothing Level": {
>        "type": "string"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/geographictypes/66?apiToken=YOURAPITOKEN"
> // MeasureId 66 corresponds to 'Age-adjusted incidence rate of Thyroid Cancer per 100,000 population'
> ```

##### Example response

> ```javascript
>[
>  {
>    "id": 506,
>    "geographicTypeId": 1,
>    "geographicType": "State",
>    "selectOptionsTypeId": 1,
>    "selectOptionsType": "No Restrictions",
>    "smoothingLevelId": 1,
>    "smoothingLevel": "No Smoothing Available"
>  }
>]
> ```

</details>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>geographicitems/{MeasureID}/{GeographicTypeId}/{GeographicRollup}</code> | List available locations for a measure</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |
> | GeographicTypeId | required | int | Unique numeric identifier for your geographic level. Obtain using <code>GET / geographictypes/{MeasureID}</code> |
> | GeographicRollup | required | int | Enter 1 to only view parent geographic items. Enter 0 to view both parent and child geographic items. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "parentGeographicId": {
>        "type": "integer"
>      },
>      "parentName": {
>        "type": "string"
>      },
>      "parentAbbrevation": {
>        "type": "string"
>      },
>      "childGeographicId": {
>        "type": "integer"
>      },
>      "childName": {
>        "type": "string"
>      },
>      "childAbbreviation": {
>        "type": "string"
>      },
>      "id": {
>        "type": "integer"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/geographicitems/226/2/0?apiToken=YOURAPITOKEN"
> // MeasureId 226 corresponds to 'Age-adjusted Incidence Rate of Thyroid Cancer per 100,000 Population over a 5-year Period', GeographicTypeId 2 corresponds to 'County', GeographicRollup 0 returns both parent (state) and child (county) geographic information
> ```

##### Example response

> ```javascript
>[
>  {
>    "parentGeographicId": 1,
>    "parentName": "Alabama",
>    "parentAbbreviation": "AL",
>    "childGeographicId": 1001,
>    "childName": "Autauga",
>    "childAbbreviation": "01001",
>    "id": 1001
>  },
>  {
>    "parentGeographicId": 1,
>    "parentName": "Alabama",
>    "parentAbbreviation": "AL",
>    "childGeographicId": 1003,
>    "childName": "Baldwin",
>    "childAbbreviation": "01003",
>    "id": 1003
>  },
>  {
>    "parentGeographicId": 1,
>    "parentName": "Alabama",
>    "parentAbbreviation": "AL",
>    "childGeographicId": 1005,
>    "childName": "Barbour",
>    "childAbbreviation": "01005",
>    "id": 1005
>  },
>  ...
>]
> ```

</details>

------------------------------------------------------------------------------------------

#### Temporal Information <a name="temporal"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>temporaltype/{MeasureID}</code> | List available temporal levels for a measure</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "temporalTypeId": {
>        "type": "integer"
>      },
>      "name": {
>        "type": "string"
>      },
>      "parentTemporalTypeId": {
>        "type": "integer"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/temporalType/1209?apiToken=YOURAPITOKEN"
> // MeasureId 1209 corresponds to 'Reported Cases per 100,000 Population'
> ```

##### Example response

> ```javascript
>[
>  {
>    "temporalTypeId": 8,
>    "name": "Day",
>    "parentTemporalTypeId": 1
>  }
>]
> ```

</details>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>temporalitems/{MeasureID}/{GeographicTypeId}/1/{GeographicItemIds}</code> | List time-ranges available for a measure at a certain location</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |
> | GeographicTypeId | required | int | Unique numeric identifier for your geographic level. Obtain using the [Geographic Information](#geographic) endpoint. |
> | GeographicItemIds | required | string | A comma-separated list of GeographicItemIds. Obtain using the [Geographic Information](#geographic) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "id": {
>        "type": "integer"
>      },
>      "parentTemporalId": {
>        "type": "integer"
>      },
>      "parentTemporal": {
>        "type": "string"
>      },
>      "parentMinimumTemporalId": {
>        "type": "integer"
>      },
>      "parentTemporalTypeId": {
>        "type": "integer"
>      },
>      "parentTemporalType": {
>        "type": "string"
>      },
>      "temporalId": {
>        "type": "integer"
>      },
>      "minimumTemporalId": {
>        "type": "integer"
>      },
>      "minimumTemporal": {
>        "type": "string"
>      },
>      "temporal": {
>        "type": "string"
>      },
>      "temporalTypeId": {
>        "type": "integer"
>      },
>      "temporalType": {
>        "type": "string"
>      },
>      "temporalTextOverride": {
>        "type": "string"
>      },
>      "parentTemporalDisplay": {
>        "type": "string"
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/temporalItems/66/1/1/13,26,48?apiToken=YOURAPITOKEN"
> // MeasureId 66 corresponds to 'Age-adjusted Incidence Rate of Thyroid Cancer per 100,000 population'. GeographicTypeId 1 corresponds to 'State'. GeographicItemIds 13,26,48 correspond to the states Georgia, Michigan, and Texas, respectively. 
> ```

##### Example response

> ```javascript
>[
>  {
>    "id": 2001,
>    "parentTemporalId": null,
>    "parentTemporal": null,
>    "parentMinimumTemporalId": null,
>    "parentTemporalTypeId": null,
>    "parentTemporalType": null,
>    "temporalId": 2001,
>    "minimumTemporalId": null,
>    "minimumTemporal": null,
>    "temporal": "2001",
>    "temporalTypeId": 1,
>    "temporalType": "Year",
>    "temporalTextOverride": null,
>    "parentTemporalDisplay": ""
>  },
>  {
>    "id": 2002,
>    "parentTemporalId": null,
>    "parentTemporal": null,
>    "parentMinimumTemporalId": null,
>    "parentTemporalTypeId": null,
>    "parentTemporalType": null,
>    "temporalId": 2002,
>    "minimumTemporalId": null,
>    "minimumTemporal": null,
>    "temporal": "2002",
>    "temporalTypeId": 1,
>    "temporalType": "Year",
>    "temporalTextOverride": null,
>    "parentTemporalDisplay": ""
>  },
>  ...
>]
> ```

</details>

------------------------------------------------------------------------------------------

#### Stratification <a name="stratification"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>stratificationlevel/{MeasureID}/{GeographicTypeId}/0</code> | List the ways data can be stratified for a measure and geographic level</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |
> | GeographicTypeId | required | int | Unique numeric identifier for your geographic level. Obtain using the [Geographic Information](#geographic) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |

##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "array",
>  "items": {
>    "type": "object",
>    "properties": {
>      "Id": {
>        "type": "integer"
>      },
>      "name": {
>        "type": "string"
>      },
>      "abbreviation": {
>        "type": "string"
>      },
>      "geographicTypeId": {
>        "type": "integer"
>      },
>      "stratificationType": {
>        "type": "array",
>        "items": {
>          "type": "object",
>          "properties": {
>            "id": {
>              "type": "integer"
>            },
>            "name": {
>              "type": "string"
>            },
>            "abbreviation": {
>              "type": "string"
>            },
>            "columnName": {
>              "type": "string"
>            }
>          }
>        }
>      }
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/stratificationlevel/66/1/0?apiToken=YOURAPITOKEN"
> // MeasureId 66 corresponds to 'Age-adjusted incidence rate of Thyroid Cancer per 100,000 population'. GeographicTypeId 1 corresponds to 'State'.
> ```

##### Example response

> ```javascript
>[
>  {
>    "id": 1,
>    "name": "State",
>    "abbreviation": "ST",
>    "geographicTypeId": 1,
>    "stratificationType": []
>  },
>  {
>    "id": 4,
>    "name": "State x Gender",
>    "abbreviation": "ST_GN",
>    "geographicTypeId": 1,
>    "stratificationType": [
>      {
>        "id": 4,
>        "name": "Gender",
>        "abbreviation": "GN",
>        "columnName": "GenderId"
>      }
>    ]
>  },
>  {
>    "id": 8,
>    "name": "State x Race/Ethnicity",
>    "abbreviation": "ST_RE",
>    "geographicTypeId": 1,
>    "stratificationType": [
>      {
>        "id": 2,
>        "name": "Race/Ethnicity",
>        "abbreviation": "RE",
>        "columnName": "RaceEthnicityId"
>      }
>    ]
>  },
>  ...
>]
> ```

</details>

-------------------------------------------------------------------------------------------

#### GetCoreHolder <a name="GetCoreHolder"></a>

<details>
 <summary><code>GET</code> <code><b>/</b></code> <code>getCoreHolder/{MeasureID}/{StratificationLevelId}/{GeographicTypeId}/{GeographicItemIds}/{TemporalTypeId}/{TemporalItemIds}/0/0</code> | Get available public health data for a measure at your specified location and timeframe, stratified as desired</summary>

##### Path parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | MeasureId | required | int | Unique numeric identifier for your desired measure. Obtain using the [Measures](#measures) endpoint. |
> | StratificationLevelId | required | int | Unique numeric identifier for your stratification level. Obtain using the [Stratification](#stratification) endpoint. |
> | GeographicTypeId | required | int | Unique numeric identifier for your geographic level. Obtain using the [Geographic Information](#geographic) endpoint. |
> | GeographicItemIds | required | string | A comma-separated list of GeographicItemIds. Obtain using the [Geographic Information](#geographic) endpoint. |
> | TemporalTypeId | required | int | Unique numeric identifier for your temporal type. Obtain using the [Temporal Information](#temporal) endpoint. |
> | TemporalItemIds | required | int | A comma-separated list of TemporalItemIds. Obtain using the [Temporal Information](#temporal) endpoint. |

##### Query parameters

> | name | required / optional | data type | description |
> |---|---|---|---|
> | apiToken | optional | string | Your unique identifier obtained from nephtrackingsupport@cdc.gov |


##### Response schema
<details>
<summary>Click to expand</summary>

> ```javascript
>{
>  "type": "object",
>  "properties": {
>    "legendResult":{
>      "type": "array",
>      "items": {}
>    },
>    "tableResult": {
>      "type": "array",
>      "items": {
>        "type": "object",
>        "properties": {
>          "id": {
>            "type": "string"
>          },
>          "dataValue": {
>            "type": "string"
>          },
>          "displayValue": {
>            "type": "string" // <---------- the measure's value
>          },
>          "year": {
>            "type": "string"
>          },
>          "groupById": {
>            "type": "string"
>          },
>          "geographicTypeId": {
>            "type": "integer"
>          },
>          "calculationType": {
>            "type": "string"
>          },
>          "noDataId": {
>            "type": "integer"
>          },
>          "noDataBreakGroup": {
>            "type": "integer"
>          },
>          "stabilityFlag": {
>            "type": "string"
>          },
>          "suppressionFlag": {
>            "type": "string"
>          },
>          "title": {
>            "type": "string"
>          },
>          "rollover": {
>            "type": "array",
>            "items": {
>              "type": "string"
>            }
>          },
>          "geo": {
>            "type": "string"
>          },
>          "geoId": {
>            "type": "string"
>          },
>          "parentGeoId": {
>            "type": "null"
>          },
>          "parentGeo": {
>            "type": "null"
>          },
>          "parentGeoAbbreviation": {
>            "type": "null"
>          },
>          "geoAbbreviation": {
>            "type": "string"
>          }
>        }
>      }
>    },
>    "tableResultWithCI": {
>      "type": "array",
>      "items": {}
>    },
>    "devDisabilitiesTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "modeledTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "healthImpactTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "airToxicTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "climateChangeTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "tableResultWithMonth": {
>      "type": "array",
>      "items": {}
>    },
>    "tableResultWithQuarter": {
>      "type": "array",
>      "items": {}
>    },
>    "dailyEstimatesTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "heatEpisodesTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "cutPointTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "pwsTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "biomonitoringTableResult": {
>      "type": "array",
>      "items": {}
>    },
>    "benchmarkInformation ": {
>      "type": "object",
>      "properties": {
>        "id": {
>          "type": "integer"
>        },
>        "measureId": {
>          "type": "integer"
>        },
>        "benchmarkId": {
>          "type": "integer"
>        },
>        "measureGeographicTypeId": {
>          "type": "integer"
>        },
>        "units": {
>          "type": "null"
>        },
>        "geographicDisplay": {
>          "type": "string"
>        },
>        "benchmarkName": {
>          "type": "string"
>        },
>        "benchmarkShortName": {
>          "type": "string"
>        },
>        "multipleSelectionActionId": {
>          "type": "integer"
>        },
>        "multipleSelectionAction": {
>          "type": "string"
>        },
>        "active": {
>          "type": "null"
>        },
>        "hasMap": {
>          "type": "boolean"
>        },
>        "hasTable": {
>          "type": "boolean"
>        },
>        "hasChart": {
>          "type": "boolean"
>        },
>        "geographicTypeDisplay": {
>          "type": "string"
>        },
>        "benchmarkFullName": {
>          "type": "string"
>        }
>      }
>    },
>    "benchmarkResult": {
>      "type": "array",
>      "items": {
>        "type": "object",
>        "properties": {
>          "id": {
>            "type": "string"
>          },
>          "dataValue": {
>            "type": "string"
>          },
>          "displayValue": {
>            "type": "string"
>          },
>          "year": {
>            "type": "string"
>          },
>          "groupById": {
>            "type": "string"
>          },
>          "geographicTypeId": {
>            "type": "integer"
>          },
>          "calculationType": {
>            "type": "null"
>          },
>          "geo": {
>            "type": "null"
>          },
>          "geoId": {
>            "type": "null"
>          },
>          "parentGeoId": {
>            "type": "null"
>          },
>          "parentGeo": {
>            "type": "null"
>          },
>          "parentGeoAbbreviation": {
>            "type": "null"
>          },
>          "geoAbbreviation": {
>            "type": "null"
>          }
>        }
>      }
>    },
>    "measureStratificationLevel": {
>      "type": "object",
>      "properties": {
>        "id": {
>          "type": "integer"
>        },
>        "measureId": {
>          "type": "integer"
>        },
>        "stratificationLevelId": {
>          "type": "integer"
>        },
>        "smoothingTypeId": {
>          "type": "integer"
>        },
>        "suppressionTypeId": {
>          "type": "integer"
>        }
>      }
>    },
>    "lookupList": {
>      "type": "object",
>      "properties": {}
>    },
>    "measureInformationDTO": {
>      "type": "object",
>      "properties": {
>        "measureInformation": {
>          "type": "object",
>          "properties": {
>            "calculationId": {
>              "type": "integer"
>            },
>            "calculation": {
>              "type": "null"
>            },
>            "calculationType": {
>              "type": "null"
>            },
>            "calculationColumnName": {
>              "type": "null"
>            },
>            "parentTemporalType": {
>              "type": "null"
>            },
>            "temporalType": {
>              "type": "null"
>            },
>            "temporalColumnName": {
>              "type": "null"
>            },
>            "temporalCount": {
>              "type": "integer"
>            },
>            "hasStability": {
>              "type": "null"
>            },
>            "smoothingType": {
>              "type": "null"
>            },
>            "precision": {
>              "type": "integer"
>            },
>            "hasConfidenceInterval": {
>              "type": "null"
>            },
>            "confidenceIntervalName": {
>              "type": "null"
>            },
>            "hasMultiMeasure": {
>              "type": "null"
>            },
>            "colorCount": {
>              "type": "integer"
>            }
>          }
>        },
>        "tableDisplayDTO": {
>          "type": "array",
>          "items": {}
>        },
>        "chartDisplayDTO": {
>          "type": "array",
>          "items": {}
>        },
>        "mapDisplayDTO": {
>          "type": "array",
>          "items": {}
>        },
>        "chartAxisExceptionDTO": {
>          "type": "array",
>        "items": {}
>        },
>        "measureCalculationExceptionDTO": {
>          "type": "array",
>          "items": {}
>        },
>        "measureDisplayException": {
>          "type": "array",
>          "items": {}
>        }
>      }
>    },
>    "tableResultClass": {
>      "type": "string"
>    },
>    "tableReturnType": {
>      "type": "string"
>    },
>    "publicAPIUrl": {
>      "type": "string"
>    },
>    "publicAPIServerUrl": {
>      "type": "string"
>    },
>    "fullPublicAPIUrl": {
>      "type": "string"
>    }
>  }
>}
> ```

</details>

##### Example cURL

> ```javascript
> curl --location "ephtracking.cdc.gov/apigateway/api/v1/getCoreHolder/66/1/1/13/1/2015/0/0?apiToken=YOURAPITOKEN"
> // MeasureId 66 corresponds to 'Age-adjusted incidence rate of Thyroid Cancer per 100,000 population'. StratificationLevelId 1 corresponds to 'State'. GeographicTypeId 1 corresponds to 'State'. GeographicItemIds 1 corresponds to the state of Georgia. TemporalTypeId 1 corresponds to 'Year'. TemporalItemIds 2015 corresponds to the year 2015.
> ```

##### Example response

> ```javascript
>{
>  "legendResult": [],
>  "dataClassificationType": null,
>  "tableResultClass": "TableResult",
>  "tableReturnType": "tableResult",
>  "tableResult": [
>    {
>      "id": "1433159",
>      "geographicTypeId": 1,
>      "geo": "Georgia",
>      "geoId": "13",
>      "geoAbbreviation": "StateAbbreviation",
>      "parentGeographicTypeId": null,
>      "parentGeo": null,
>      "parentGeoId": null,
>      "parentGeoAbbreviation": null,
>      "calculationType": "Age Adjusted Rate",
>      "temporalTypeId": 1,
>      "temporal": "2015",
>      "temporalDescription": "Single Year",
>      "temporalColumnName": "ReportYear",
>      "temporalRollingColumnName": "RollingYearCount",
>      "temporalId": 2015,
>      "minimumTemporal": null,
>      "minimumTemporalId": null,
>      "parentTemporalTypeId": null,
>      "parentTemporalType": null,
>      "parentTemporal": null,
>      "parentTemporalId": null,
>      "year": "2015",
>      "dataValue": "12.6",
>      "displayValue": "12.6", // <---------- the measure's value
>      "groupById": "1",
>      "noDataId": -1,
>      "hatchingId": -1,
>      "hatching": null,
>      "suppressionFlag": "0",
>      "noDataBreakGroup": 0,
>      "confidenceIntervalLow": null,
>      "confidenceIntervalHigh": null,
>      "confidenceIntervalName": null,
>      "standardError": null,
>      "standardErrorName": null,
>      "secondaryValue": null,
>      "secondaryValueName": null,
>      "descriptiveValue": null,
>      "descriptiveValueName": null,
>      "includeDescriptiveValueName": null,
>      "categoryId": 0,
>      "category": null,
>      "categoryName": null,
>      "rollover": [
>        "Age Adjusted Rate: 12.6"
>      ],
>      "title": "Georgia",
>      "confidenceIntervalDisplay": "",
>      "standardErrorDisplay": "",
>      "confidenceIntervalLowName": "",
>      "confidenceIntervalHighName": "",
>      "secondaryValueDisplay": "",
>      "parentMinimumTemporal": null,
>      "parentMinimumTemporalId": null
>    }
>  ],
>  "healthImpactTableResult": [],
>  "climateChangeTableResult": [],
>  "dailyEstimatesTableResult": [],
>  "heatEpisodesTableResult": [],
>  "pmTableResult": [],
>  "regionPMTableResult": [],
>  "cwsTableResult": [],
>  "sampleSizeTableResult": [],
>  "stateMetadataTableResult": [],
>  "dailyTemperatureTableResult": [],
>  "benchmarkInformation": {
>    "id": 27,
>    "measureId": 66,
>    "benchmarkId": 9,
>    "measureGeographicTypeId": 1,
>    "units": null,
>    "geographicDisplay": "National",
>    "benchmarkName": " Rate per 100,000 population",
>    "benchmarkShortName": " Rate per 100,000 population",
>    "multipleSelectionActionId": 1,
>    "multipleSelectionAction": "Continue",
>    "active": true,
>    "geographicTypeDisplay": "National Benchmark",
>    "benchmarkFullName": "National  Rate per 100,000 population",
>    "title": "National",
>    "hasTable": true,
>    "hasChart": true,
>    "hasMap": true
>  },
>  "benchmarkResult": [
>    {
>      "id": "447057",
>      "geographicTypeId": 1,
>      "geo": null,
>      "geoId": null,
>      "geoAbbreviation": null,
>      "parentGeographicTypeId": null,
>      "parentGeo": null,
>      "parentGeoId": null,
>      "parentGeoAbbreviation": null,
>      "calculationType": "Age Adjusted Rate",
>      "temporalTypeId": 1,
>      "temporal": "2015",
>      "temporalDescription": "Single Year",
>      "temporalColumnName": "ReportYear",
>      "temporalRollingColumnName": "RollingYearCount",
>      "temporalId": 2015,
>      "minimumTemporal": null,
>      "minimumTemporalId": null,
>      "parentTemporalTypeId": null,
>      "parentTemporalType": null,
>      "parentTemporal": null,
>      "parentTemporalId": null,
>      "year": "2015",
>      "dataValue": "14.8",
>      "displayValue": "14.8",
>      "groupById": "1",
>      "rollover": [
>        "National  Rate per 100,000 population: 14.8"
>      ],
>      "parentMinimumTemporal": null,
>      "parentMinimumTemporalId": null
>    }
>  ],
>  "standard": [],
>  "aggregateResult": null,
>  "lookupList": {},
>  "measureInformationDTO": {
>    "measureInformation": {
>      "calculationId": 0,
>      "calculation": null,
>      "calculationTypeId": null,
>      "calculationType": null,
>      "calculationColumnName": null,
>      "parentTemporalType": null,
>      "temporalType": null,
>      "temporalTypeId": 0,
>      "temporalColumnName": null,
>      "temporalCount": 0,
>      "hasStability": null,
>      "smoothingType": null,
>      "precision": 0,
>      "hasConfidenceInterval": null,
>      "confidenceIntervalName": null,
>      "hasStandardError": null,
>      "standardErrorName": null,
>      "hasSecondaryValue": null,
>      "secondaryValueName": null,
>      "hasDescriptiveValue": null,
>      "descriptiveValueName": null,
>      "primaryLegendTitle": null,
>      "secondaryLegendTitle": null,
>      "colorCount": null,
>      "defaultBreakGroupCount": null,
>      "defaultDataClassificationTypeId": null,
>      "units": null,
>      "message": null,
>      "messageURL": null,
>      "activeMessage": null,
>      "categoryTypeId": null,
>      "temporalTextOverride": null
>    },
>    "tableDisplayDTO": [],
>    "chartDisplayDTO": [],
>    "mapDisplayDTO": [],
>    "chartAxisExceptionDTO": [],
>    "measureCalculationExceptionDTO": [],
>    "measureDisplayException": []
>  },
>  "measureStratificationLevel": {
>    "id": 0,
>    "measureId": 0,
>    "stratificationLevelId": 0,
>    "smoothingTypeId": 0,
>    "suppressionTypeId": 0
>  },
>  "publicAPIUrl": "/apigateway/api/v1/getCoreHolder/66/1/1/13/1/2015/0/0",
>  "publicAPIServerUrl": "https://ephtracking.cdc.gov",
>  "fullPublicAPIUrl": "https://ephtracking.cdc.gov/apigateway/api/v1/getCoreHolder/66/1/1/13/1/2015/0/0"
>}
> ```

</details>
