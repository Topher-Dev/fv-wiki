# FightView API Documentation

## Overview

The **FightView API** provides centralized access to our extensive database of MMA fighters, fights, events, user profiles, predictions, and related content. It's designed to serve and work in conjunction with the FightView client JS application and aims to be accessible for external developers in the future.

## API Access Point

All API requests are routed through the following endpoint:

`https://api.fightview.app/arc.php`

## Authentication

Some resources require a valid **JWT token** for access. Include this token as a URL parameter to authenticate your requests. Public routes are accessible without a token.

**Format:**
`?token=YOUR_JWT_TOKEN`

## Request Format

Requests must include parameters specifying the **controller** (the resource you're accessing) and the **service** (the action you're requesting).

**Format:**
`?token=YOUR_JWT_TOKEN&controller=RESOURCE_NAME&service=ACTION_NAME`

## Controllers and Services

### Fighters Controller

- **List Fighters** (Public)
  - `?controller=fighter&service=read_list`
  - Retrieves a list of fighters with basic information.

- **Get Fighter Details** (Public)
  - `?controller=fighter&service=read_one&fighter_id=ID`
  - Retrieves detailed information about a specific fighter.

### Fights Controller

- **List Fights** (Public)
  - `?controller=fight&service=read_list`
  - Retrieves a list of fights.

- **Get Fight Details** (Public)
  - `?controller=fight&service=read_one&fight_id=ID`
  - Retrieves detailed information about a specific fight.

### Events Controller

- **List Events** (Public)
  - `?controller=event&service=read_list`
  - Retrieves a list of MMA events.

- **Get Event Details** (Public)
  - `?controller=event&service=read_one&event_id=ID`
  - Retrieves detailed information about a specific event.

### Predictions Controller

- **Get Fight Predictions** (Restricted)
  - `?controller=prediction&service=fight_prediction&fight_id=ID`
  - Provides AI-driven predictions for specific fights. Requires authentication.

### User Profiles Controller (Restricted)

- **Get User Profile**
  - `?controller=user&service=read_one&user_id=ID`
  - Retrieves the profile of a registered user. Requires authentication.

- **Update User Profile**
  - `?controller=user&service=update_one&user_id=ID`
  - Updates user profile information. Requires authentication.

## Rate Limiting

The API enforces rate limiting to ensure fair usage. Details about limits and how they are applied are provided upon obtaining an API key.

## Error Handling

The API uses app-level error handling to indicate the success or failure of requests. Error messages provide additional context about the issue.

A status code of 0 indicates a successful request; a status code of non-0 indicates a failed request.

## Example Request (List Fighters)

`https://api.fightview.app/arc.php?controller=fighter&service=read_list`

## Example Response (List Fighters)

```json
{
  "status": 0,
  "message": "Fighters retrieved",
  "data": [
    {
      "id": "fighter123",
      "name": "John Doe",
      "weightClass": "Lightweight",
      "record": "12-3-0",
      "country": "USA"
    }
  ]
}
{
  "status": 1001,
  "message": "The provided value for 'weightClass' is not recognized. Please use one of the predefined weight class values."
  "guilty_param": "weightClass",
  "guilty_value": "Super Heavy",
}

