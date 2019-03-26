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

## Lanching CERI clearance channel for GS

[The project](https://w.amazon.com/bin/view/CNTechAGS/Projects/New_CERI/) launched the CERI clearance channel for GS in CN.

In the project, our team built the Unified Clearance Platform(the following is called UCP) to cover both CERI and PCR clearance mode. I delivered two core services in the project - **XBorderDataAggregatorService** and **XBorderOrderInfoDeclarationService** - from design to implementation.

- **XBorderOrderInfoDeclarationService** The service serves to submit order data to customs agents. 
- **XBorderDataAggregatorSerivce** The service is the data provider of clearance data. It integrates 6+ external dependcy services.  The service also severs as a data source of [Unified Settlement Platform](https://w.amazon.com/bin/view/CNTech/LastMile/USP/) of payment team.

In the project, we faced the extensible problem. As business required, UCP has to onboard more than 1 customs agents(e.g. SHANGHAI, GUANGZHOU etc.). Each customs agent has its own commnication channel and protocol. So the service has to be easy to expand to new local customs office. It's not acceptable to spend the same effort as current project. Another challenge is that the consistency issue of protocol's implementaion. In any system, more interfaces means more communciating issue. The services need to provide fault tolerance and analysis tools to solve operation issues.

The service **XBorderOrderInfoDeclarationService** defined a plugin-like framework to solve External extensible issue. The framework encapuslates the low level communication functions and defines the high level interfaces for pluin extension. It supports both sync submitting and trap receipt capability with customs agents. For onboarding a new customs office, SDE only need to parse the specific protocol and map it to system's abstract concepts. In the service, we designed the retry mechinism by leveraging the SQS's DLQ to provide fault tolerance and simplified the maintain operations as redriving the DLQ. With the service **XBorderDataAggregatorSerivce**, internal services are wrappered in an unified way to provide source data. Based on the strategy pattern, the service picked the right source clearance data for each order.

After project launch, 93% GS packages switched to CERI clearacne channel from PCR channel. It saved both time and cost. Average clearance time is reduced to less 0.5 day from PCR's 3 days. Average duty rate is reduced to 11.2% from PCR's 30%. With the framework design, the onboarding of GUANGZHOU and NINGBO and ZHONGTONG in afterward projects becomes as easy as expected. Each plugin was finished by a junior SDE with in 3 weeks.

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


### FTZ project \<TBD>

## Discussion with Wanp

### 2019-03-25 first
1. All projects need to add result section
2. Need improve challenge description
3. Add FTZ project, get feedback from Qingping for Wireless.
4. Need improve visiblity in future
   1. Talk with payment PM
   2. Talk with compliance Carter
