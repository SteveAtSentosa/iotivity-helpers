IoTivity Core Overview
======================

## General Capabilities

* A common semantic / data model
* Common communication protocols for discovery and connectivity
* Common security and identification mechanisms

## History

* OIC (Open Interconnect Consortium) founded 2014
  - Founded by Samsung and Intel
  - Focused on IoT standards based on CoAP (Constrained Application Protocol)
* Name changed to Open Connectivity Foundation (OCF) in 2016
  - Microsoft, Qualcomm, Electrolux joined
* Currently over 300 member companies.

## Architectural overview

#### Key Principals

**Resource Oriented Architecture**
* Entities in the physical world are represented as Resources
* Interactions are via REST based messaging
  CRUDN (creativity, retrieve, update, delete, notify)

**Three Tier Architecture**
```
  Resource Model
  Restful Operations
  Mapping of Rest Operations to concrete elements (CoAp)
```

**CoAP**

Applicaiton Protocol targted 'Constrained Devices'
* Constrained devide defined in [RFC_7228](https://tools.ietf.org/html/rfc7228)
* CoAP protocols and HTTP integration defined in [RFC_7252](https://tools.ietf.org/html/rfc7252)

The CoAP was designed with the following in mind:
* URI and content-type support
* Support for the discovery of resources provided by known CoAP services.
* Simple subscription for a resource, and resulting push notifications.
* Simple caching based on max-age.


#### Key Features

**Identification and addressing:** Defines the identifier and addressing capability

**Discovery:** Processes for discovering available devicds/endpoints and resources

**Resource model:** Mechanisms for manipulating resources represnting entities

**CRUDN:** Scheme for interactions betwen Client and Server

**Messaging:** Message protocols for RESTful CRUDN operations (CoAP is the primary messaging protocol)

**Device management:**. device provisioning, initial setup,monitoring and diagnostics.

**Security:** Authentication, authorization, and access control for secure access to Entities.




## Identification and Addressing

## Resource

## CRUDN

## Network and Connectivitty

## Endpoint

## Functional Interactions
#### Onboarding, Provisioning and Configuration
#### Resource discovery
#### Notification
#### Device management
#### Scenes
#### Introspection

## Messaging
#### Mapping of CRUDN to CoAP
#### CoAP serialization over TCP
#### Payload Encoding in CBOR

## Security
**See Security Overview**
