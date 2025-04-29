---
title: "From chaos to clarity: Effective documentation for evolving architectures"
description: "
The clear and effective documentation is essential for quickly-evolving software. 
As systems grow and change, maintaining an accurate representation of the architecture becomes more challenging. 
In this post I would like to introduce you some powerful tools and approaches for achieving clarity. 
By integrating decision records and C4 Modeling can enhance documentation strategy and make sure that the stakeholders have the common understanding of the system design.
"
date: 2025-04-29 08:00:00 +/-TTTT
categories: [architectures, documentation]
tags: [system-design, documentation, teamwork]     # TAG names should always be lowercase
image:
  path: /assets/img/plans.jpeg
---

## C4 Modeling: A Framework for Clarity
### Overview of the C4 Model

The C4 model, developed by Simon Brown, provides a structured approach to visualizing software architecture at different abstraction levels. 
Basically there are four levels:
- Level 1: Context Diagram
This diagram is about how the system interacts with external entities, such as users and external systems. 
It provides a high-level view of the system's environment.
  
![Context level](/assets/img/context-diagram.svg)
- Level 2: Container Diagram
This aims to break down the system into its major containers (applications, services, databases) and give an overview
about how they communicate with each other.

![Container level](/assets/img/container-diagram.svg)
- Level 3: Component Diagram
On this level the diagram details the components within a specific container, showing their interactions and responsibilities.
  
![Component level](/assets/img/component-diagram.svg)
- Level 4: Code Diagram (Optional)
The 4. level  provides insight into the internal structure of a component, often using UML class diagrams but can also be represented in other ways.
  
![Code level](/assets/img/code-diagram.svg)

### Benefits of Using C4 for Architecture Documentation

- Clarity: C4 diagrams provide a clear visual representation of the architecture, making it easier to understand for technical and non-technical stakeholders as well.
- Scalability: The model scales well with the complexity of the system. Teams are able to document both small and large architectures effectively.
- Communication: C4 serves as a common language for discussing architecture among team members, facilitating better communication and collaboration.

##  Architectural Decision Records (ADRs)

### Definition and Purpose of ADRs

Architectural Decision Records (ADRs) are a way to capture architectural decisions made during the development process. 
Each ADR documents a specific decision, including the context, the decision itself, and the consequences of that decision. 
This practice helps teams maintain a historical record of architectural choices and the rationale behind them.

### Structure of an ADR
A well-structured ADR typically includes the following components:

- Title: A brief description of the decision.
- Status: The current status (e.g., proposed, accepted, deprecated).
- Context: The background information that led to the decision.
- Decision: The actual decision made.
- Consequences: The implications of the decision, both positive and negative.

You can see a typical example for the ADR [here.]( https://github.com/arpadharcsa/technical-blog-tutorials/blob/main/documentation/structurizr/documentation/decision-records/0001-record-architecture-decisions.md)
### Benefits of Maintaining ADRs

- Lightweight Documentation: ADRs are simple to create and maintain, making them a practical choice for teams of all sizes.
- Knowledge Sharing: ADRs facilitate knowledge transfer within the team, especially for new members.
- Alignment: They help ensure that all team members are aligned on architectural decisions and their implications.
- Historical Reference: ADRs provide a historical context for why certain decisions were made, which can be invaluable during future discussions or when revisiting architectural choices.

## Integrating C4 and ADRs

The combination of the C4 model and Architectural Decision Records (ADRs) provides a powerful approach to documenting and maintaining software architecture. Together, they ensure that both the "what" and the "why" of the architecture are well-documented.

## Core Concept of Structurizr

Structurizr is a tool designed to visualize and document software architecture using the C4 model. The core idea is to create living, dynamic documentation that evolves alongside the system it represents. With the usage of this tool the documentation remains relevant and up-to-date, providing a clear and accurate architecture overview.

### Using the DSL as the Single Source of Truth

The Structurizr DSL can serve as a single source of truth for documenting and visualizing software architecture. By defining the system landscape, components, and relationships in a structured format, the DSL ensures consistency and alignment across all documentation artifacts.


I have created an example DSL which is available [here.]( https://github.com/arpadharcsa/technical-blog-tutorials/blob/main/documentation/structurizr/documentation/system-landscape.dsl)


**ADR Integration**: 

The line `!adrs ./decision-records` links the defined software system  to the ADRs, ensuring that the rationale behind architectural decisions is directly accessible from the Structurizr.

```
system = softwareSystem "Software System" {
            !adrs ./decision-records
        }
```


**Component-Level Documentation**: 

The line `!docs ./components/webapp/detailed-design.md` binds detailed design documentation to the `Web Application` container, providing in-depth insights into its architecture.

```
webapp = container "Web Application" {
                !docs ./components/webapp/detailed-design.md
                controller = component "Controller" {
                    description "Handles HTTP requests and responses"
                    technology "Spring MVC"
                }
                service = component "Service" {
                    description "Business logic layer"
                    technology "Spring Service"
                }
                repository = component "Repository" {
                    description "Data access layer"
                    technology "Spring Data JPA"
                }
            }
```

**Dynamic Diagrams**: 

The `dynamic system requestDataFlow` section defines a dynamic view of the system, illustrating how data flows between components during a specific feature execution.

```
 dynamic system requestDataFlow {
            title "Request data from database feature"
            user -> system.webapp "Requests data via API"
            system.webapp -> system.db "Queries for data using"
            autoLayout lr
        }
```

**Deployment Environment**: 

The `deploymentEnvironment "Production"` section specifies the production setup, including deployment nodes and container instances, offering a clear view of the runtime architecture.

```
production = deploymentEnvironment "Production" {
            deploymentNode "Server 1" {
                containerInstance system.webapp
                deploymentNode "Database Server" {
                    containerInstance system.db
                }
            }
            deploymentNode "Server 2" {
                containerInstance system.webapp
                deploymentNode "Database Server" {
                    containerInstance system.db
                }
            }
        }
```

### Running Structurizr Locally with Docker

To run Structurizr locally and interact with the DSL, you can use the following `docker-compose.yaml` file:

```yaml
services:
  structurizr:
    image: structurizr/lite:latest
    environment:
      STRUCTURIZR_WORKSPACE_FILENAME: system-landscape
    volumes:
      - ./documentation:/usr/local/structurizr
    ports:
      - "8080:8080"
```

This setup allows you to:

- Host Structurizr Lite locally.
- Map the `documentation` folder to the container, enabling Structurizr to access the DSL and related files.
- Access the application via `http://localhost:8080` to view and interact with the generated diagrams and documentation.

## Conclusion

Clear documentation for evolving architectures is important, but it's not self-explanatory how to achieve it. In this post, I shared my experience with C4 modeling and ADRs. These types of tools help us to reach a common clarity of our software product.
A working skeleton, including the Structurizr DSL and Docker setup, is available on the [Github.]( https://github.com/arpadharcsa/technical-blog-tutorials/tree/main/documentation/structurizr)

Feel free to use it! :)