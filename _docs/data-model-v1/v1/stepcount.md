---
layout: defaults
title: Step Count V1
published: true
---

# Step Count

A step object represents the number of steps a user has taken in a period of time. These are typically recorded by wearable activity trackers (such as Jawbone or Fitbit) or standalone fitness apps and imported into Tidepool

An step count record includes information on the time and provenance of when the step count was recorded, as well as the number of steps the user has taken during the recording period

## Object
A typical step count object is as follows:

~~~json
  {
    "type": "stepCount",  

    "time": "2015-10-14T04:00:50.000Z",
    "timezoneOffset":-240,
    "conversionOffset":0,
    "deviceId": "jawbone-A1234", 
    "uploadId": "0001", 

    "datapoint": {  
          "header":{
            "acquisition_provenance": {
            "source_name": "Jawbone UP API", 
            "modality": "sensed", 
            "external_id": "43t7Tx40joGTZSuJe1H4I8KzpgjxT7a7" 
          }
              
          },
          "body": {
            "effective_time_frame": {
            "time_interval": {
              "start_date_time": "2015-10-14T00:00:50-04:00", 
              "end_date_time": "2015-10-14T23:14:51-04:00" 
            }
          },
          
          "step_count": 7851 
          
        }
    }
  }
~~~

### Common Fields
For the definitions of `type`, `time`, `timezoneOffset`, `conversionOffset`, `deviceId`, `uploadId` see common fields in the data model documentation

### Datapoint
The `datapoint` field contains the information about the step count recording including its provenance and number of steps taken by the user. The format of data point objects are based on Open mHealth Schemas. For further reference :http://www.openmhealth.org/documentation/#/schema-docs/overview and http://www.openmhealth.org/documentation/#/schema-docs/schema-library/schemas/omh_step-count

#### Header
The `header` field contains the details on how the event was captured:

`source_name`: (required) describes the device or app that this data was reteived from

`modality`: (optional) Can be either 'sensed' or 'self-reported' depending on whether the step count was recorded by the device/app or manually entered by the user 

`external_id`: (optional) An ID that identify the step count recording in the source system.

#### Body
The `body` field adds details about the steps recorded for the the user:

`start_date_time`: Timestamp for the start of the recording period. Includes a time zone

`end_date_time`: Timestamp of the end of the recording period. Includes a time zone
          
`step_count`: The number of steps taken by the user during the recording period

