# FightView Database Design

## Overview

This document provides a comprehensive outline of the FightView database schema, designed to store and manage data related to MMA fighters, fights, events, user interactions, and multimedia content. The schema emphasizes flexibility, simplicity, and robustness, supporting efficient data retrieval for the PHP API, facilitating complex ML/AI analytical queries, and ensuring data integrity and scalability.

## Schema Overview

The FightView database includes tables for `fighter`, `fight`, `eventx`, `user`, `prediction`, `user_prediction`, `event_type`, `fighter_stats`, and `content_media`. The design fosters an optimized structure for query performance and data integrity.

### System Fields for Every Table

To maintain consistency, integrity, and facilitate soft deletes, auditing, and accountability across the database, the following system fields will be included in each table:

- `is_deleted`: BOOLEAN -- Indicates if the record has been logically deleted.
- `created_by`: INTEGER -- References `user(user_id)` to track who created the record.
- `updated_by`: INTEGER -- References `user(user_id)` to track who last updated the record.
- `deleted_by`: INTEGER -- References `user(user_id)` to track who deleted the record.
- `created_at`: TIMESTAMP -- Automatically captures when the record was created.
- `updated_at`: TIMESTAMP -- Automatically updates when the record is modified.
- `deleted_at`: TIMESTAMP -- Captures when the record was logically deleted.

### Entities and Relationships

#### Fighter

- **Table Name**: `fighter`
- **Columns**:
  - `fighter_id` SERIAL PRIMARY KEY
  - `name` VARCHAR(255)
  - `weight_class` VARCHAR(50)
  - `country` VARCHAR(50)
  - `record` VARCHAR(50) -- Format: wins-losses-draws
  - Additional biographical and statistical information as required.

#### Fight

- **Table Name**: `fight`
- **Columns**:
  - `fight_id` SERIAL PRIMARY KEY
  - `event_id` INTEGER REFERENCES `eventx(event_id)`
  - `fighter_id_one` INTEGER REFERENCES `fighter(fighter_id)`
  - `fighter_id_two` INTEGER REFERENCES `fighter(fighter_id)`
  - `winner_id` INTEGER REFERENCES `fighter(fighter_id)` NULLABLE
  - `method` VARCHAR(100)
  - `round` INTEGER
  - `time` VARCHAR(50)

#### Eventx

- **Table Name**: `eventx`
- **Columns**:
  - `event_id` SERIAL PRIMARY KEY
  - `name` VARCHAR(255)
  - `date` DATE
  - `location` VARCHAR(255)
  - `organization` VARCHAR(100)
  - `event_type_id` INTEGER REFERENCES `event_type(event_type_id)`

#### User

- **Table Name**: `user`
- **Columns**:
  - `user_id` SERIAL PRIMARY KEY
  - `username` VARCHAR(255) UNIQUE
  - `email` VARCHAR(255) UNIQUE
  - `password_hash` VARCHAR(255)
  - `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP

#### Prediction

- **Table Name**: `prediction`
- **Columns**:
  - `prediction_id` SERIAL PRIMARY KEY
  - `fight_id` INTEGER REFERENCES `fight(fight_id)`
  - `predicted_winner_id` INTEGER REFERENCES `fighter(fighter_id)` NULLABLE
  - `confidence_score` DECIMAL
  - `model_version` VARCHAR(50)

#### User Prediction

- **Table Name**: `user_prediction`
- **Columns**:
  - `user_prediction_id` SERIAL PRIMARY KEY
  - `fight_id` INTEGER REFERENCES `fight(fight_id)`
  - `user_id` INTEGER REFERENCES `user(user_id)`
  - `predicted_winner_id` INTEGER REFERENCES `fighter(fighter_id)` NULLABLE
  - `confidence_score` DECIMAL
  - `prediction_time` TIMESTAMP DEFAULT CURRENT_TIMESTAMP

#### Event Type

- **Table Name**: `event_type`
- **Columns**:
  - `event_type_id` SERIAL PRIMARY KEY
  - `type_name` VARCHAR(255)
  - `description` TEXT

#### Fighter Stats

- **Table Name**: `fighter_stats`
- **Columns**:
  - `fighter_stat_id` SERIAL PRIMARY KEY
  - `fighter_id` INTEGER REFERENCES `fighter(fighter_id)`
  - `stat_name` VARCHAR(255)
  - `stat_value` DECIMAL
  - `as_of_date` DATE

#### Content Media

- **Table Name**: `content_media`
- **Columns**:
  - `content_id` SERIAL PRIMARY KEY
  - `content_type` VARCHAR(50) -- e.g., 'video', 'image', 'article'
  - `source_url` VARCHAR(255)
  - `description` TEXT
  - `related_fighter_id` INTEGER REFERENCES `fighter(fighter_id)` NULLABLE
  - `related_fight_id` INTEGER REFERENCES `fight(fight_id)` NULLABLE
  - `related_event_id` INTEGER REFERENCES `eventx(event_id)` NULLABLE
  - `publish_date` TIMESTAMP

## Data Integrity and Relationships

- **Foreign Keys** ensure referential integrity between related entities, including system fields such as `created_by`, `updated_by`, and `deleted_by` linking back to the `user` table for accountability.
- **Indexes** on frequently queried columns, including system fields like `created_at` and `updated_at`, improve performance.
- **Constraints** enforce data quality, uniqueness, and the integrity of system fields.

## Supporting ML/AI Analysis

- The inclusion of system fields, especially timestamps, aids in the temporal analysis and ensures that ML/AI models can be trained on historical data while being aware of data lineage.
- **Feature Tables** like `fighter_stats` are designed to pre-aggregate data for ML/AI analysis, enriched by system fields to track data changes over time.
- **Data Pipeline Integration** is planned for automated extraction, transformation, and loading (ETL) processes, enabling real-time data updates and analysis. This integration considers system fields for incremental updates and logical deletes.

