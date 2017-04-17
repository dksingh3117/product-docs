.. _middleware:


Structure
@@@@@@@@@

We use a three-tier classification structure for APIs.

* System - APIs that directly access databases and organize the data into functional domains (e.g. the “Person” data element)

* Process - APIs that deal with processes and orchestration (e.g. the “Order Test” process)

* Experience - APIs that combine process and/or System APIs to expose data directly to an app developer (e.g. a web portal experience API that abstracts a FHIR system API and an order test process API with a light-weight RESTful interface). The Experience API can be used to create a front end experience without understanding complex system domains or business logic encapsulated within process APIs.

Example
@@@@@@@

.. image:: /_static/stack/mulecatalyst.png

This is an example published by MuleSoft of a three-tier architecture for health care. In it, data are abstracted from complex EHR systems like Epic into a canonical model that is represented via a set of FHIR REST APIs. Then, experience APIs are layered on top to provide better experiences for both patients and clinicians.

**1. System Layer**

System APIs abstract away the complexity of EHRs and other core systems of record from the data’s end user, while providing downstream insulation from any interface changes or rationalization of those systems.

Assets Included:

* CRM FHIR System API | Salesforce Implementation Template

* CRM FHIR System API | RAML Definition

* EHR FHIR System API | EHR Implementation Template*

* EHR FHIR System API | RAML Definition

* Fitness FHIR System API | Fitbit Implementation Template

* Fitness FHIR System API | RAML Definition

**2. Process Layer**

Process APIs decouple business processes that interact with and shape data from the source systems where the data originated. For example, the “schedule appointment” process contains logic that is common across multiple entities, which can be called by product, geography, or channel-specific parent services.

Assets Included:

* Onboarding Process API | Implementation Template

* Appointments Process API | Implementation Template

* Appointments Process API | RAML Specification

* EHR to CRM Sync Process API | Implementation Template

* Fitness Data Sync Process API | Implementation Template

* HL7 Event Handler | Implementation Template (Coming soon)

**3. Experience Layer**

Experience APIs are the means by which data can be reconfigured so that it is most easily consumed by its intended audience, all from a common data source, rather than setting up separate point-to-point integrations.

Assets included:

* Web Portal Experience API | Implementation Template

* Web Portal Experience API | RAML Specification

* Salesforce Experience API | Implementation Template

Supporting Assets:

* HL7 Connector

* 12 FHIR APIs | RAML Specification