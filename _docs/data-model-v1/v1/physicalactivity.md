---
layout: defaults
title: Physical Activity V1
published: true
---

# Physical Activity

A physical activity object represents the an activity such as a running, cycling, etc that a user performs. These are typically recorded by wearable activity trackers (such as Jawbone or Fitbit) or standalone fitness apps (such as Strava or Runkeeper) and imported into Tidepool

An Physical Activity record includes information on the time and provenance of the activity as well as information about the activity such as the type of activity, the distance covered, calories burned, etc

## Object
A typical physical activity object is as follows:

~~~json
  {
    "type": "physicalActivity",

    "time": "2015-10-24T16:17:50.000Z", 
    "timezoneOffset":-240,
    "conversionOffset":0,
    "deviceId": "runkeeper-A1234", 
    "uploadId": "0001", 

    "datapoint": {  
          "header":{
            "acquisition_provenance": {
            "source_name": "Runkeeper HealthGraph API", 
            "modality": "sensed", 
            "external_id": "/fitnessActivities/679963655" 
          }
              
          },
          "body": {
            "effective_time_frame": {
            "time_interval": {
              "start_date_time": "2015-10-24T12:17:50-04:00", 
              "duration": {
                "unit": "sec",
                "value": 148.311  
              }
            }
          },
          "activity_name": "Running",  
          "distance": { 
              "unit": "m",
              "value": 10.3929068646607 
            }
          },
          "kcal_burned": { 
            "unit": "kcal",
            "value": 7     
          },
          "reported_activity_intensity": "moderate" 
          
        }
  }
~~~

### Common Fields
For the definitions of `type`, `time`, `timezoneOffset`, `conversionOffset`, `deviceId`, `uploadId` see common fields in the data model documentation

### Datapoint
The `datapoint` field contains the information about the physical activity including its provenance and details of the physical activity. The format of data point objects are based on Open mHealth Schemas. For further reference :http://www.openmhealth.org/documentation/#/schema-docs/overview and http://www.openmhealth.org/documentation/#/schema-docs/schema-library/schemas/omh_physical-activity

#### Header
The `header` field contains the details on how the event was captured:

`source_name`: (required) describes the device or app that this data was reteived from

`modality`: (optional) Can be either 'sensed' or 'self-reported' depending on whether the activity was recorded by the device/app or manually entered by the user 

`external_id`: (optional) An ID that identify the activity reading in the source system.

#### Body
The `body` field adds details about the physical activity the user performed:

`start_date_time`: Timestamp of when the activity started. Includes a time zone

`duration`: The length of time the activity was performed for
          
`activity_name`: The name of the activity e.g. 'running'

`distance`: (optional) distance covered during an activity

`kcal_burned`: (optional) kCal burned during the activity

`reported_activity_intensity`: (optional) Intensity level reported by the user - one of "light", "moderate" or "vigorous"
