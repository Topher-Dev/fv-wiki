# Feature Breakdown for FightView

This document outlines the FightView application's modular design, emphasizing the functionality and interaction between modules. The app is structured around primary view modules, navigation modules, and utility modules, each serving specific roles to create a seamless user experience.

## Primary View Modules

These modules are central to the app's functionality, directly rendering content into the main container based on user interactions.

### 1. **Fight List Module**
- **Description**: Displays fights from the active or selected event. By default, this module shows the next upcoming or currently live event.
- **Navigation Access**: Accessed through the central button on the bottom navigation bar.
- **Active Event Selection**: The active event can be changed using the Top Search Module, dynamically updating the fight list to reflect the selected event.

### 2. **Fight Preview/Summary Module**
- **Description**: Offers a detailed preview of upcoming fights or a summary of past fights.
- **Access Points**:
  - Selecting a fight from the Fight List Module.
  - The adjacent button on the bottom navigation bar.
  - Selecting from the Top Search Module
### 3. **Fighter Details Module**
- **Description**: Shows comprehensive information about fighters.
- **Access Points**:
  - Selecting a fighter from the Fight Preview/Summary Module.
  - The third button on the bottom navigation bar.
  - Top Search Module

## Navigation Modules

Designed to facilitate user movement within the app, these modules are always present and provide essential navigational functionality.

### 1. **Bottom Navigation Bar**
- **Functionality**: Houses buttons that switch between the primary view modules.

### 2. **Top Search Module**
- **Functionality**: Enables users to search for events, fights, or fighters. Utilizing this module allows users to select a different event, which updates the Fight List Module to display fights from the chosen event.
- **Active Event Display**: Directly underneath the search bar, the selected event is displayed with an icon to indicate it's actively selected, providing users with a clear indication of the context for the displayed fight list.

## Utility Modules

Utility modules offer additional functionalities and settings, enhancing the overall user experience without directly rendering into the main container.

### 1. **Menu Module**
- **Access**: Activated by the top-right hamburger button, this overlay menu includes user profile access and a list of utility options.

#### Sub-Modules within the Menu Module
- **Membership Management**: Handles subscription payments for non-members via Stripe and displays membership status for existing members.
- **Registration/Login**: Provides user authentication functionalities.

## Naming Conventions

To maintain clarity and consistency across the development team, modules are categorized based on their roles and interaction patterns:

- **Primary View Modules**: Directly influence the main content area, dynamically updating based on user selections.
- **Navigation Modules**: Facilitate easy access to the primary views and are constantly available for user interaction.
- **Utility Modules**: Include the Menu Module and its sub-modules, offering additional features and settings accessible through the main interface.

This naming convention distinguishes between the modules that actively change the main display (`Primary View Modules`), those that assist in navigating the app (`Navigation Modules`), and those providing extra functionalities (`Utility Modules`). It aims to streamline communication and documentation, ensuring a shared understanding of the app's structure.

## Module Styling

- [web/client/css/core_modules.css] Contains styling related to *Navigation & Utility Modules*
- [web/client/css/view_modules.css] Same as core but for *Primary View Modules* 

## Client App
- [app.js] Singleton object that manages the client application
- [core_modules.js] Contains HTML Templates and Logic with the Component class for *Navigation & Utility Modules*
- [view_modules.js] Same as core but for *Primary View Modules*
- [modal_modules.js]
- [utility.js]
- [animation.js]
- [ui.js]
- [reef.js]
- [auth.js]
- [network.js] 
- [events.js]
- [token.js]
 