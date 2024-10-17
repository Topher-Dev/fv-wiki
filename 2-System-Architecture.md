# Architecture for FightView

This document provides an overview of the FightView application architecture, detailing the core components and their interactions. The architecture is designed to be scalable, secure, and maintainable, supporting the application's requirements and future growth.

## Overview

FightView is built on a modern technology stack, leveraging PHP, JavaScript, PostgreSQL, Python, and various tools and libraries to deliver a comprehensive experience for MMA enthusiasts. The PHP API adopts a Model-Controller architecture pattern to separate concerns, enhance code reusability, and simplify maintenance.

## Core Components

### Utilities (FV) for Application Admins
- **Location**: `fv/sbin`
  - `db.sh` 
      **backup**: Saves a backup of the database .gz, location based on config details
      **restore [path]**: Loads a backup database based on arg path
      **list**: Lists tables
      **sync**: Downloads all backups from the remote machine to the local
      **reverse_sync**: Uploads all backups from local machine to remote server
      **version**: Prints the version of the database
      **upgrade**: Updates the version of the database with upgrade file
      **password [email] [password]**: Updates person password.
      **reset**: 
  - `install.sh (full)`: Installs all dependencies and configures the environment.
  - `config.sh` (export|apache): Makes configuration values available to different parts of the system.
  - `model.sh`: Generates the corresponding model (json) and controller (php) files from the database table. Updates existing files to reflect database changes.
  - `app.sh`: Provides a snapshot of the application's status by checking various servers/services.

### Web Server (Apache2)
- **Functionality**: Hosts the FightView application, handling HTTP/HTTPS requests, routing to PHP scripts, and serving static assets.
- **Configuration**: Managed by `sbin/install.sh apache`.

### Caching (Memcached)
- **Functionality**: Employs Memcached for efficient data caching, reducing database load and enhancing content delivery speed.
- **Implementation**:
  - **Query Caching**: Automatically caches and periodically clears commonly executed query results, like fighter rankings.
  - **Object Caching**: Caches frequently accessed objects such as model.json files for quick retrieval.
- **Configuration**: Features smart cache invalidation to ensure users always receive up-to-date information.
- **Integration**: Seamlessly integrated with the PHP backend, abstracting caching mechanisms from business logic.

### Backend (PHP)
- **Location**: `fv/web/server`
- **MC Architecture**:
  - **Model**: Manages database interactions, business logic, and data validation.
  - **Controller**: Handles user requests, fetching data from the model and delivering it in JSON format.
- **API**: Offers RESTful endpoints for client-side interactions, enabling features like fight listings and user profile management.

### Database (PostgreSQL)
- **Functionality**: Central repository for all application data, including user profiles, fighter details, and event information.
- **Design**: Utilizes a relational schema for data integrity and supports complex queries for advanced analytics.

### Data Collection
- **Location**: `/data/collector`
  - `ini.py`: Main script for initiating data collection processes.
  - `sources.py`: Definitions of data sources and retrieval logic.
  - `transform.py`: Data transformation and normalization logic.
  - `load.py`: Script for loading data into the database.
  - `cronjob.txt`: Cron job setup instructions.

### Data Analyzer
- **Location**: `/data/analyzer`
  - `predictor.py`: Core script for generating AI/ML predictions.
  - `/models/`: Directory for storing trained model files.
    - `fighter_performance.model`
    - `fight_outcome.model`
  - `analysis.py`: Utilities for data analysis and feature extraction.
  - `train.py`: Scripts for training AI/ML models with collected data.

### Libraries and Common Utilities
- **Location**: `/lib`
  - `DatabaseWrapper.py`: PHP class for database interaction abstraction.
  - `CacheManager.py`: Utility for managing Memcached operations.
  - `Authenticator.py`: Common authentication functions and JWT handling.
  - `Utilities.py`: Miscellaneous helper functions used across the application.

### Client-APP (JavaScript)
- **Location**: `fv/web/client`
- **Technologies**: Uses modern JavaScript (ES6+) to create a dynamic and responsive Single Page Application (SPA).
- **Interaction**: Engages with the PHP backend via AJAX, updating the user interface dynamically to ensure a smooth user experience.
- **Initialization**: The `index.php` file prompts the client's browser to download necessary JS files, initializing the application upon completion.

### Security

Security within the FightView application is multifaceted, covering various aspects to ensure the integrity, confidentiality, and availability of the system and its data.

- **Authentication & Authorization**: Utilizes JSON Web Tokens (JWT) for secure user authentication and session management. This method ensures that each request to the server is accompanied by a valid token, which is verified for authenticity before any data is processed or returned. The tokens are designed with a limited lifespan to reduce the risk of token theft and misuse.

- **Data Protection**: Employs HTTPS for all communications between the client and the server, ensuring that data in transit is encrypted and protected from interception. Additionally, rigorous input validation is implemented to prevent common attacks such as SQL injection and cross-site scripting (XSS). Output encoding is also used to prevent XSS attacks by ensuring that any data rendered on the client-side is encoded and safe to display.

- **Firewall Configuration (UFW)**: The Uncomplicated Firewall (UFW) is configured to allow only necessary traffic to the FightView servers. This simple yet effective tool provides an additional layer of security by blocking unauthorized access attempts and mitigating potential attacks.

- **Security by Ports**:
  - **Port 80 (HTTP)**: Traffic on port 80 is redirected to port 443 (HTTPS) to enforce secure connections. This ensures that all communications are encrypted, providing confidentiality and integrity of the transmitted data.
  - **Port 443 (HTTPS)**: Exclusively used for encrypted traffic, ensuring that all data exchanged between the client and the server is secure. HTTPS not only protects against eavesdropping but also provides authentication of the accessed website.

- **Intrusion Detection System (IDS)**: Implements an IDS to monitor network and system activities for malicious activities or policy violations. Any detected activity or violation is logged and reported to an administrator, providing an early warning of a potential security issue.

By integrating these security measures, FightView ensures a robust defense against various cyber threats, protecting both the users' data and the integrity of the application itself. The commitment to security is ongoing, with continuous monitoring, testing, and updating to adapt to the evolving cyber threat landscape.

## Conclusion

The FightView application architecture is meticulously crafted to offer a robust, scalable, and engaging platform for MMA fans. By adhering to industry-standard practices and architectural patterns, FightView ensures a secure, maintainable, and high-performing application.
