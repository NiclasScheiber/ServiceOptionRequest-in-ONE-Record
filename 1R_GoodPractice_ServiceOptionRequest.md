# Enabling the booking of generic services in ONE Record

- [Enabling the booking of generic services in ONE Record](#enabling-generic-services-in-one-record)
  * [Basic Information on this document](#basic-information-on-this-document)
    + [Objective](#objective)
    + [Target audience](#target-audience)
    + [Geographical coverage](#geographical-coverage)
    + [Creators](#creators)
    + [Continous development and availability](#continous-development-and-availability)
    + [Use and reference](#use-and-reference)
    + [Publication date, version and history](#publication-date--version-and-history)
  * [Dependencies](#dependencies)
    + [Standards applied](#standards-applied)
    + [ONE Record Server Implementation used](#one-record-server-implementation-used)
    + [Other Software products used](#other-software-products-used)
  * [Assumptions](#assumptions)
  * [Solution approach](#solution-approach)
  * [Solution in current environment](#solution-in-current-environment)
- [Data use and target process](#data-use-and-target-process)  
- [API use](#api-use)
    + [Technical setting](#technical-setting)
    + [Basic API-Features used](#basic-api-features-used)
  * [Results / Summary](#results---summary)
  * [Additional comments / FAQs](#additional-comments---faqs)


## Basic Information on this document

### Objective 
The purpose of this document is to provide a Good Practice for booking and using addtional, generic services through the ONE Record ecosystem.

### Target audience
This document can be used by any party with the interest of providing the booking and using addtional, generic services through the ONE Record ecosystem. 

### Geographical coverage
As there are no legal or operational restrictions, the solution can be used worldwide.

### Creators
This document is the outcome of a ONE Record pilot project with the "Digitales Testfeld Air Cargo" by the German air cargo community. Parties/Persons involved were:

Frankfurt University of Applied Sciences/IAT, Niclas Scheiber

Fraunhofer IML, Oliver Ditz

Lufthansa Cargo, Philipp Billion

Fraport, Athanasios Papakarmesis

Fraport, Andreas Quick

### Continous development and availability

This document is to be used and continously developed, even if the current major stakeholders should move to other topics. Thus a "handover" in Github is planned if responsibilities should shift.

### Use and reference

This Good Practice is free to access and use. If you use it, please refer to this document explicitly plus provide a link to the Github repository as source. This will ensure know-how-transfer and transparency.

### Publication date, version and history

Publication date, version and history should be provided by the Github version control system and not be duplicated here.

## Dependencies

### Standards applied

The ONE Record business ontology version as of APR 13, 2022 was used [Working draft Ontology of 2022APR13](https://github.com/IATA-Cargo/ONE-Record/blob/bbe86e364b04d6a6279f0ab6e9ee47e1905ec9c4/working_draft/ontology/IATA-1R-DM-Ontology.ttl).

The ONE Record API and security specification draft witout a version as of APR 13, 2022 was used (no link available yet).

### ONE Record Server Implementation used

(no ONE Record server implementation yet)

### Other Software products used

## Assumptions

One or more stakeholders, hereafter the booker, want to book an additional service for any given logistics object with another stakeholder, hereafter the provider. Alternatively, the provider might want to have a booker book a service to ensure the logistics object in processual possession of a booker is fed into the correct process of the provider.

The booker has knowledge of the offered services and knows the respective URIs. The booker requests a particular service from a provider.

The provider has the power to accept or reject a service. The provider can have the booker select from multiple service options.

The status of a service (acceptance/rejection) has to be visible to booker and provider alike. Visibility of a booked service may be delegated to other stakeholders of the transport chain.

## Solution approach

The ONE Record data model follows two principles: Piece-centricity and physics-orientation. Piece-centricity is self-explanatory, as every information is linked with the piece as the lowest available transportation object. Physics-orientation means that the linking strucutre of ONE Record follows the structures of the physical world (for more details, please refer to [ONE Record Github Repository](https://github.com/IATA-Cargo/ONE-Record).

Building on the principle of physics-orientation, generic services shall be bookable for any given physical object. Offering and booking services on piece-level is however, the recommended best practice, satisfying the principle of piece-orientation.

The suggested data model addition follows closely the implementation of handling bookings with air carriers with its various distribution logistics objects as per business ontology version of ONE Record linked in the standards applied section of this document. 

The overall vision is that stakeholders can use ONE Record as API for handling any additional process-relevant service in bilateral and multilateral environments. These services may be monetized, but can also be "free of charge" and be used to transmit a location-specific piece of information required to have a provider feed a given physical unit into the correct process.

It has to be noted that, as of this version of this document, pricing and rating features are not yet modelled akin to the distribution logistic objects as to keep the approach clean and simple. Nevertheless, if demand arises through fitting user stories, an addition is possible.

## Solution in current environment

In the legacy environment, there is no world-wide generally accepted standard to transmit service requests for any physical unit of air freight. Instead, various location- or stakeholder-specific solutions are used. These include print-outs, phone calls, telefax, e-mails, and own handling system to handling system APIs.

# Data use and target process

A ***serviceOptionRequest*** can be added for any logistic object (LO) consolidating (physical) freight, namely ***piece***, ***ULD***, ***transportMovement*** and ***shipment***. A ***serviceOptionRequest*** is created by a booker and links a static (@Philipp: Korrekter Begriff für solche LOs wie auch TransportMeans?) ***serviceProduct*** hosted by the provider, requiring a PATCH by the provider. Upon acceptance, the provider creates n ***serviceOption***s for all valid bookable options for the booker, each linking the ***serviceOptionRequest*** to keep the direct connection to the LO for which a service is booked, and the ***serviceProduct*** as reference.

|   	|Explanation
|---	|---	
|Focus LOs  	|***piece***, ***ULD***, ***transportMovement***, ***shipment***
|New LOs  	|***serviceOptionRequest***, ***serviceProduct***, ***serviceOption***
|Example| A forwarder (booker) wants ULD 123 to be routed to handover point X upon unloading on apron by GHA (provider)
|Purpose| Booking generic services and letting stakeholders know if a generic service is booked
|Provider of data	| Object Owner (Focus LO), Booker (***serviceOptionRequest***) and provider (***serviceProduct***, ***serviceOption***)
|Target Group | Any ONE Record user involved with physical processes

The following diagram shows the relevant data fields in the ONE Record data model:

![DataModel](docs/ServiceRequestDiagram (3)_reduced.drawio.svg)


Hereafter, the minimum required relevant LOs and data fields are briefly described.

## piece LO - !!!

Holds general data about a given piece which may be included for determining whether a specific service can be provided or not or calculating service prices. ***skeletonBy*** holds the company (@Philipp: richtig?) that has created a skeleton. Links towards any number of serviceOptionRequests (1:n link).

### Data field: skeletonBy

If created from a shipment for the purpose for having a particular service performed on a piece it will hold the ***company*** which created it and likely is the booker.

## ULD LO

Holds general data about a given ULD which may be included for determining whether a specific service can be provided or not or calculating service prices. Links towards any number of ***serviceOptionRequests*** (1:n link).

## transportMovement LO

Holds general data about a given transport segment which may be included for determining whether a specific service can be provided or not or calculating service prices. Links towards any number of ***serviceOptionRequests*** (1:n link).

## shipment LO

Holds general data about a given shipment which may be included for determining whether a specific service can be provided or not or calculating service prices. Links towards any number of ***serviceOptionRequests*** (1:n link).

## serviceOptionRequest LO

Holds the initial request of a booker with a link towards a ***serviceProduct*** (n:1 link) of a provider and a link to a ***piece***, ***ULD***, ***transportMovement***, and ***shipment*** (n:1 link). Links to n ***serviceOption***s (1:n link) upon creation by the provider. At the minimum, it needs to include information about the service requested, the booker (or: the party who authorized the booker). Those are to be modelled as the following data fields: ***serviceName***, ***requestor***, ***requestAuthorizedBy***. 

Additionally, a ***requestStatus*** can be added to more quickly iterate through all ***serviceOptionRequest***s of a given LO.

### Data field: serviceName

Holds the name of the requested service as a *String*. Used in conjunction with data field ***requestStatus*** to quickly iterate through LOs and fetch booked services.

### Data fields: requestor and requestAuthorizedBy

Holds the *Company* booking a service and, if the booker is booking on behalf of another company, optionally the *Company* on which behalf the request was started.

### Optional data field: requestStatus - to be discussed

Holds the status of the request as a yet unspecified data type. As a minimum, it needs to show the following statuses: accepted by provider, rejected by provider, cancelled by booker, pending by booker, pending by provider. POSTed by booker, status and link PATCHed by booker and provider.

An implementation as *Integer* could look like:
0 - pending by provider
1 - pending by booker
2 - accepted by provider
3 - rejected by provider
4 - cancelled by booker

## serviceProduct LO

Static object. Holds any service offered by a provider company. Links to multiple ***serviceOptionRequests*** from bookers (1:n link) and multiple ***serviceOption***s (1:n link) generated for any ***serviceOptionRequest*** linked to it. Includes datafields ***serviceName*** and ***serviceProvider***. POSTed and hosted by the service provider.

Versioning is necessary when the service offered changes and is, as of this draft, to be handled via audit trail.

Additionally, data fields ***serviceType*** and ***serviceDescription*** may be added to categorize services in company-specific or yet-to-be-defined industry-wide categories and to provide bookers with more in-detail human-readable information respectively.

### Data field: serviceName

Holds the name of the requested service as a *String*. Used to validate with linked ***serviceOptionRequest***s and to provide a short description of the service to the booker.

### Data field: serviceProvider

Holds the *Company* providing the service.

### Optional data fields: serviceType and serviceDescription

Hold the type of the service and a detailed, human-readable description of the service as *String*s.

## serviceOption LO

Holds necessary information about a service option to be selected by the booker. At the minimum, has to include information about its status, matching with a request and information about the consequences of booking this specific option. Links to one ***serviceProduct*** (n:1 link) of the provider and one ***serviceOptionRequest*** (n:1 link) of the booker. Includes datafields ***serviceStatus***, ***requestMatchInd*** and ***serviceDescription***. POSTed by provider.

Additionally, ***validFrom*** and ***validTo*** data fields might be added to handle cases where offers have a limited time of validity. However, no user story exists just yet.

### Data field: serviceStatus
Holds the service status as *String* (offered, confirmed, ...)

### Data field: requestMatchInd - !!!!
Holds *Boolean* indicating that (@Philipp?)

### Data field: serviceDescription
Describes the service offered with this particular option as a *String*. 

Best practice: Should not be identical to any other serviceDescription of other ***serviceOption LO***s linked to the same ***serviceOptionRequest LO***.

### Optional data fields: validFrom and validTo
Holds, if required, the times between the option is valid as *Datetime*.

# API use

### Technical setting

### Basic API-Features used

## Results / Summary

## Additional comments / FAQs

## Offene Themen

Wie wissen auf welchen Flug sich was bezieht?

ULD Problematik!!

### How do we deal with missing piece information?

TBD if necessary
