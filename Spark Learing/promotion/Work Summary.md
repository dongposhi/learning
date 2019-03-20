## Requirement of document
- clarify what was delivered personally (versus leading others to deliver), what was challenging/complex about the work, and how success was measured.

- Technical Artifacts Demonstrate technical and/or scientific expertise. These include scientific research papers, models, algorithm design documents, technical specifications, working mockups/prototypes, code samples, code review commentary (both given and received), architecture/design documents, patent submissions, etc.
- Work Artifacts Demonstrate important functional skills. These include PR/FAQs, narratives, Product Roadmaps, OP1 documents, MBR/QBR materials, functional specifications, UX designs, presentation decks, broadcast videos, project proposals/ROI case studies, wikis, resource planning documents, etc.

# Work Summary

## Design work of CERI clearance expanding on third party crossboard(the following is called 3PCB) business

GS team has built unified clearance platform(the following is called UCP, it provides customs clearance service for crossboarder business) for CN GS and FTZ business. The project was to expand to 3P business so that 3P crossboard sellers can take advantage of UCP's CN-specific CERI clearance channel. It avoids the 3P sellers collected customers' KYC directly which caused lots of order cancellation due to trust issue. It can save cost and shorten the duration of clearance(3~5 days) for sellers.

The main challenge of the project comes from unprecise input and different data stream. 3PCB sellers have their own clearance agents and fulfillment workflow. It has different data stream and workflow with GS and FTZ business. 3PCB's messages may be uploaded in half-automatic way, so it is error-prone. UCP need provide fault tolerance design to cover all kinds of abnormal scenario and defined a workflow to handle the cases automatically.

I finished the technical design on UCP and lead the implementation. The work involved UCP's 4 core services XBorderDataAggregatorService, XBorderOrchestrationService, XBorderTransDispaterEventService, CustomerDataTransmissionService. The design defined the workflow of data exchange, error handling with 3rd-party clearance agents.

### Techinical Artifacts
- Design document
  -  https://w.amazon.com/index.php/CNTechAGS/Projects/New%20CERI/3P%20Onboard%20Plan
- Code review
  - https://cr.amazon.com/r/7830923/
  - https://cr.amazon.com/r/7891481/
  - https://cr.amazon.com/r/7761655/

## Design and Implementation of UCP's core components - XBorderDataAggregatorService and XBorderOrderInfoDeclarationService
GS team built the [UCP(Unified Clearance Platform, it provides customs clearance service for crossboarder business)](https://w.amazon.com/bin/view/CNTechAGS/Projects/New_CERI/#XBorderDataAggregatorService) from scratch to support various clearance channels. I took two core services design and implementation.

- **XBorderOrderInfoDeclarationService** The service serves to submit order data to customs agents. The service defined a framework of communciating with them. The framework encapuslates the low level commnication functions and defines the high level interfaces for pluin extension. It supports both sync submitting and trap receipt capability with customs agents. SDE can implments the plugin for each clearance agents which has its own protocol and data requirement with low cost.


  I finished the SHANGHAI CBT agent. And other workmates finished other plugins on GUANGZHOU, NINGBO based on the framework within short time (less than 1 person month).

- **XBorderDataAggregatorSerivce** The service is the data provider of clearance data. It involves 3 main systems (checkout, shipment, payment) in Retail system, 6+ services as PCE/OMS, HOPS, VBS, KYC, TokenatorIntegrationService, AddressService. The service integrated different data sources. The service also severs as a data source of [Unified Settlement Platform](https://w.amazon.com/bin/view/CNTech/LastMile/USP/)

### Techinical Artifacts

- Design documents
  - https://w.amazon.com/index.php/XBorderOrderInfoDeclarationService
  - https://w.amazon.com/index.php/CNTechAGS/Projects/New_CERI/XBorderDataAggregatorService
- Code review
  - https://cr.amazon.com/r/6776174/
  - https://code.amazon.com/packages/XBorderOrderInfoDeclarationService/commits/85abebe3d7ce2a05a646adc069c745b44a106c3a# 


## Project: Payer Purchaser KYC validation - KYCManagementService new API implementation and UCP backend service design

As compliance requirement from CN customs house, crossboard retail website need provide Payers' KYC information to do validation with the customers' footprint in payment processor. The [project](https://w.amazon.com/index.php/CNTechAGS/Projects/ppc) is to collect payer's KYC information. Beside the collecting KYC via front-end, UCP (Unified Clearance Platform) need tune the workflow for it.

I tooked 2 parts work in the project. 
- Define and implement the necessary APIs in KYCManagementService to support the payer's KYC managment.
  - Existing KYCManagementService has a set of API to maintain the KYC storage and relation mapping. But the code is not well designed. Lots of duplicated logic make it not easy to read and maintain. In the work, I didn't follow the original way but re-design the code to implement the new business logic. 
- Design the workflow change in UCP to adapt the new compliance requirement.
  - Design doc focused on the workflow adjustment in UCP. As the UCP is an online system. The design defined concreate deployment plan to avoid introducing additional maunual operation.


### Techinical Artifacts
- https://w.amazon.com/bin/view/CNTechAGS/Projects/New_CERI/UCP/PayerKYCValidation/KYCManagementServiceAPIChange/

- https://code.amazon.com/reviews/CR-3819852
- https://w.amazon.com/bin/view/CNTechAGS/Projects/New_CERI/transportChannelDesign/