---
layout: post
title: Creating objects to store in Bluemix Mobile Data
tags: [ios, bluemix, mobile]
---
Using IBM [Bluemix](http://bluemix.net) to create mobile applications provides a great service for storing data in the cloud.

Below is how I set-up my own objects to be stored from an iPhone app:

* Create a new Objective-C object that extends `IBMDataObject` and implements `IBMDataObjectSpecialization`

   ```objc
@interface Conference : IBMDataObject <IBMDataObjectSpecialization>
   ```
* Add properties for each of the fields that will be stored in the database

   ```objc
@property (copy, nonatomic) NSString *conferenceTitle;
@property (copy, nonatomic) NSString *presentationTitle;
@property (copy, nonatomic) NSString *numberOfAttendees;
@property (copy, nonatomic) NSString *date;
@property (copy, nonatomic) NSString *imageData;
   ```

* Define the properties as `dynamic` in the implementation

   ```objc
@dynamic conferenceTitle;
@dynamic presentationTitle;
@dynamic numberOfAttendees;
@dynamic date;
@dynamic imageData;
   ```

* Override the `dataClassName` function

   ```objc
+ (NSString *)dataClassName {
    return @"Conference";
}
   ```

This object can then be used with any of the API calls that add or retrieve data from the database.
