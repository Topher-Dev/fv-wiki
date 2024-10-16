# FightView Development Roadmap

This document outlines the FightView Development Roadmap, serving as a guide for the planned development phases. Our aim is to ensure a structured and efficient approach to the development of the FightView web application.

## Development Phases

### Phase 1: Core Infrastructure Setup
- **Objective**: Establish the foundational technology stack by completing and utilizing the tools in `fv/sbin`.
all files require selecting a method.eg:
 `sbin/config export`
executing the file without a parameter: `sbin/config` will print out the available methods for use

- **Components**:
  - [ ] [config.sh](2-Application-Architecture#configsh)
  - [ ] [install.sh](2-Application-Architecture#installsh)
  - [ ] [app.sh](2-Application-Architecture#appsh)
  - [ ] [db.sh](2-Application-Architecture#dbsh)
  - [ ] [model.sh](2-Application-Architecture#modelsh)
- **Complete Criteria**: Running `install.sh` should setup and configure the app environment. `app.sh` should confirm the success and running status of all core infrastructure.

### Phase 2: Database Design and Implementation
- **Objective**: Design and implement the database schema for managing MMA fighters, fights, events, and user data.
- **Components**:
  - [ ] Create and normalize database tables. ([etc/ini.sql](4-Database-Design))

### Phase 3: Data Collector & Analyzer
- **Objective**: Implement systems for automated data collection and analysis to support dynamic content and insights.
- **Components**:
  - [ ] Develop the Data Collector for aggregating MMA data. ([data/collector](2-Application-Architecture#data-collection))
  - [ ] Create the Data Analyzer for AI/ML predictions based on collected data. ([data/analyzer](2-Application-Architecture#data-analyzer))

### Phase 4: Backend Development
- **Objective**: Develop the server-side logic, API endpoints, and integrate the database.
- **Components**:
  - *Note: Use `sbin/model.sh` to initialize a model/controller based on a table. The table must exist.*
  - [ ] Develop Data Models ([web/server/models](5-Backend-Api))
  - [ ] Develop RESTful API ([web/server/controllers](5-Backend-Api))

### Phase 5: Frontend Development
- **Objective**: Develop the user interface and user experience components.
- **Components**:
  - [ ] Implement app views/modules ([web/client/js](3-Feature-Breakdown))
  - [ ] Style the application ([web/client/css](3-Feature-Breakdown))

### Phase 6: Deployment and Monitoring
- **Objective**: Deploy the application to the production environment and set up monitoring tools.
- **Components**:
  - Configure continuous integration and deployment pipelines.
  - Set up logging and monitoring tools for performance and error tracking.
  - Establish a protocol for regular updates and maintenance.
