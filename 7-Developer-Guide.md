# Developer's Guide for FightView

Welcome to the FightView Developer's Guide. This document provides essential information to get started with development, contribute effectively to the project, and maintain a high-quality codebase.

## Getting Started
#### Initial Setup Instructions

1. **Get a Fresh Installation of Ubuntu 22.04**
   - Ensure you're starting with a clean and updated installation of Ubuntu 22.04 to avoid any compatibility issues. If you're setting up a new environment, download the Ubuntu 22.04 LTS image from the official [Ubuntu website](https://ubuntu.com/download/desktop) and follow the installation guide.
   - After installation, update your system to make sure all the default software is up to date. Open a terminal and run:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

2. **Install Git**
   - Git is essential for version control and collaborating on the FightView project. Install Git by opening your terminal and entering:
     ```bash
     sudo apt update
     sudo apt install git -y
     ```
   - Verify the installation by checking the Git version:
     ```bash
     git --version
     ```
     This command should return the installed version of Git, confirming it's correctly installed on your system.

3. **Clone the FightView Repository**
   - With Git installed, you're ready to clone the FightView repository to your local machine. Navigate to the directory where you wish to place the project and execute:
     ```bash
     git clone https://github.com/Topher-Dev/fightview.git
     ```
### Environment Setup

After cloning the FightView repository, the next step is to set up your development environment by running the installation script included in the repository. This script will install all necessary dependencies, configure your environment, and prepare the application for use.

#### Running the Installation Script

1. **Navigate to the Project Directory**
   - Before running the installation script, make sure you are in the root directory of the FightView project you cloned earlier.
     ```bash
     cd fv
     ```

2. **Execute the Install Script**
   - Run `sbin/install.sh` script with the `full` argument to start the full installation and configuration process.
     ```bash
     sudo ./sbin/install.sh
     ```
     - Respond to prompts for necessary configuration details:
       - app_runmode dev|prod (choice)
       - jwt_secret Application Private Key: Used for JSON Web Tokens (JWT). ( Input Validated for length)
       - database_password: For PostgreSQL database access. (Input Validated for complexity)

3. **Execute the system script**
   - Run `sbin/system.sh`, this will validate that all core technologies / servers are running and expected
     ```bash
     sudo ./etc/system.sh
     ```

By following these steps, you will have set up the core infrastructure and environment necessary for developing the FightView application. This process ensures that all developers have a standardized setup, minimizing compatibility issues and streamlining the development workflow.


## Code Contribution Guidelines
- Use Git for version control. Clone the project repository to your local machine for development.
- Set up the local development server with Apache2 and configure it to serve the FightView application.
- Install PostgreSQL and create a local database that mirrors the production schema for testing.

## Code Contribution Guidelines

### Workflow
- **Branch Naming**: Feature or bug branches should follow the naming convention `feature/<feature-name>` or `bugfix/<bug-description>`.
- **Pull Requests**: Submit pull requests (PRs) against the `develop` branch. Ensure your PRs are well-documented and include any necessary tests.

### Coding Standards
- Adhere to the snake_case naming convention for PHP and JavaScript files.
- Write clean, maintainable code and follow the established patterns in the project.
- Ensure code compatibility with PHP 7.4 and above.

### Testing
- Write unit and integration tests for all new features and bug fixes.
- Utilize PostgreSQL for database-related testing.
- Test your changes locally before submitting a PR to ensure stability and compatibility.

## Documentation
- Update the wiki with any new features or changes to the development process.
- Document any new code, especially APIs and interfaces, to maintain clarity and ease of use for other developers.

## Community and Support
- Join our development discussions on [platform-specific communication tool].
- For help or questions, refer to the "Support" section or reach out to the community.

This guide is a starting point for contributing to FightView. By following these guidelines, you'll help maintain the quality and consistency of the codebase and contribute to the project's overall success.
