---
description: Learn more about the MTP concept and their relationships to each other...
---

# Functions

### Relationships

![MTP Relationships](../../.gitbook/assets/explorer-v2-diagrams-3-.jpg)

### Core Concepts

#### Photos

Photos are street-level images of a location.

* Photo belongs to 1 user
* Photo belongs to 1 sequence
* Photo has viewpoint count
  * Any user \(except owner can mark TRUE/FALSE\)
* Photo has &gt;=0 object tags
  * Mapillary object tags
  * MTP object tags
* [Photo has linked Mapillary photo record](https://www.mapillary.com/developer/api-documentation/#images)

#### Sequences

Sequences are collections of photos continuously captured by a user at a given location.

* Sequence belongs to 1 user
* Sequence has &gt;= 1 photos
* Sequence has like count
  * Any user \(except owner can mark TRUE/FALSE\)
* Sequence can belong to 1 or more Tour owned by Sequence owner
* Sequence object has metadata:
  * A title
  * A description \(optional\)
  * A transport type \(parent / child\)
  * &gt;=0 descriptive tags
  * Camera make
  * Captured date
  * Created date \(uploaded to Mapillary\)
  * [Mapillary Sequence key](https://www.mapillary.com/developer/api-documentation/#sequences)
  * Coordinate properties
    * Mapillary Photo keys
  * Is panorama

**Tours**

 Tours are collections of Sequences that have some relationship curated by Sequence owner. 

* Tour belongs to 1 user
* Tour has &gt;= 1 sequences
  * Sequence must belong to tour creator
* Tour has like count
  * Any user \(except owner can mark TRUE/FALSE\)
* Tour has
  * A title
  * A description \(optional\)
  * &gt;=0 descriptive tags

**Guidebooks**

Guidebooks are collections of any Mapillary photos with annotations to provide more information.

* Guidebook belongs to 1 user
* Guidebook has &gt;= 1 photos
  * Photo does not need to belong to imported MTP sequence. Can be any Mapillary image
* Guidebook has like count
  * Any user \(except owner can mark TRUE/FALSE\)
* Guidebook has &gt;=0 descriptive tags

**Leaderboard**

Leaderboards show users 

* Leaderboards have two types
  * Photos uploaded \(by user\): counts number of photos uploaded by user
  * Viewpoints recieved \(by user\): counts number of viewpoints user
* Leaderboards have filters
  * By transport type \(parent/child\)
  * By camera make \(make/model\)
  * By date
* Leaderboards can be
  * Global: considers all photos and has filters any user can modify
  * Challenges: are fixed and related to settings defined when challenge was created \(no filters for user\)

**Challenges**

A challenge encourages user to capture photos in a defined location, and/or using a defined transport type,  and/or using a defined camera over a defined time period.

**Hire**

Any user can create a for hire listing to promote their paid street-level mapping services.

Hire listings are owned by user, but have no relationship to any other objects.

