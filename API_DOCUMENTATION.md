# Bracelet-connecte

## Project Overview

The main objective of the project is to enable **real-time measurement and monitoring of vital signs**.

This **wearable device** is designed to address the need for **health prevention** and **daily monitoring of vital indicators**. It continuously measures:

- Oxygen saturation (**SpO₂**)
- Heart rate
- Body temperature

The readings are **instantly displayed** on a screen integrated into the wristband and **automatically and securely transmitted to the cloud** using Wi-Fi.

A **dedicated mobile application** allows:
- The **user** (for self-monitoring), or  
- **Authorized supervisors** (such as doctors or family members)

to:
- View the data  
- Access historical records  
- Receive **immediate alerts** in case of anomalies or risky situations (such as a fall or irregular heart activity)

The system also ensures **data privacy and security** for all personal information.

## API Endpoints

### Authentication

- **POST** `/token`
  - **Description:** OAuth2 compatible token login, get an access token for future requests

### Devices

- **POST** `/devices/register`
  - **Description:** Register a new device.
- **GET** `/devices/`
  - **Description:** Retrieve devices for the current user.
- **GET** `/devices/{device_id}`
  - **Description:** Get a specific device by ID.
- **PUT** `/devices/{device_id}`
  - **Description:** Update a device.
- **DELETE** `/devices/{device_id}`
  - **Description:** Delete a device.
- **GET** `/devices/{device_id}/metrics`
  - **Description:** Get all metrics for a specific device.

### Issues

- **POST** `/issues/`
  - **Description:** Create new issue.
- **GET** `/issues/`
  - **Description:** Retrieve issues.
- **GET** `/issues/{issue_id}`
  - **Description:** Get a specific issue by ID.
- **PUT** `/issues/{issue_id}`
  - **Description:** Update an issue.
- **DELETE** `/issues/{issue_id}`
  - **Description:** Delete an issue (soft delete).

### Metrics

- **POST** `/metrics/batch/`
  - **Description:** Create new metrics for the current device.
- **GET** `/metrics/`
  - **Description:** Retrieve all metrics (admin only).
- **GET** `/metrics/{metric_id}`
  - **Description:** Get a specific metric by ID.
- **DELETE** `/metrics/{metric_id}`
  - **Description:** Delete a metric (admin only).

### Summary

- **GET** `/users/{user_id}/metrics/summary`
  - **Description:** Get aggregated metrics for a user.
- **GET** `/users/{user_id}/metrics/data`
  - **Description:** Get raw metric data for a user, filtered by metric_type.

### Users

- **POST** `/users/`
  - **Description:** Create new user.
- **GET** `/users/`
  - **Description:** Retrieve users.
- **GET** `/users/{user_id}`
  - **Description:** Get a specific user by ID.
- **PUT** `/users/{user_id}`
  - **Description:** Update a user.
- **DELETE** `/users/{user_id}`
  - **Description:** Delete a user (soft delete).
- **POST** `/users/verify-email`
  - **Description:** Verify user email with a 6-digit code.
- **POST** `/users/forgot-password`
  - **Description:** Send password reset code.
- **POST** `/users/reset-password`
  - **Description:** Reset user password with a 6-digit code.

## Database Models

### User

- `id` (UUID): Primary key.
- `name` (String): User's name.
- `email` (String): User's email address (unique).
- `email_verified_at` (DateTime): Timestamp of email verification.
- `hashed_password` (String): Hashed password.
- `is_admin` (Boolean): Flag for admin users.
- `created_at` (DateTime): Timestamp of creation.
- `updated_at` (DateTime): Timestamp of last update.
- `deleted_at` (DateTime): Timestamp of soft deletion.
- `verification_code` (String): 6-digit email verification code.
- `verification_code_expires_at` (DateTime): Expiration timestamp for verification code.
- `password_reset_code` (String): 6-digit password reset code.
- `password_reset_code_expires_at` (DateTime): Expiration timestamp for password reset code.

### Device

- `id` (UUID): Primary key.
- `name` (String): Device name.
- `serial_number` (String): Device serial number (unique).
- `api_key` (String): API key for the device (unique).
- `model` (String): Device model.
- `firmware_version` (String): Firmware version of the device.
- `is_active` (Boolean): Flag for active devices.
- `registered_at` (DateTime): Timestamp of device registration.
- `user_id` (UUID): Foreign key to the `users` table.
- `created_at` (DateTime): Timestamp of creation.
- `updated_at` (DateTime): Timestamp of last update.
- `deleted_at` (DateTime): Timestamp of soft deletion.

### Metric

- `id` (UUID): Primary key.
- `metric_type` (Enum): Type of the metric (e.g., "temperature", "humidity").
- `value` (Float): Value of the metric.
- `unit` (String): Unit of the metric.
- `sensor_model` (String): Model of the sensor.
- `timestamp` (DateTime): Timestamp of the metric reading.
- `user_id` (UUID): Foreign key to the `users` table.
- `device_id` (UUID): Foreign key to the `devices` table.
- `created_at` (DateTime): Timestamp of creation.
- `updated_at` (DateTime): Timestamp of last update.
- `deleted_at` (DateTime): Timestamp of soft deletion.

### Issue

- `id` (UUID): Primary key.
- `issue_type` (String): Type of the issue.
- `description` (String): Description of the issue.
- `severity` (Enum): Severity of the issue.
- `detected_at` (DateTime): Timestamp of when the issue was detected.
- `resolved` (Boolean): Flag for resolved issues.
- `user_id` (UUID): Foreign key to the `users` table.
- `created_at` (DateTime): Timestamp of creation.
- `updated_at` (DateTime): Timestamp of last update.
- `deleted_at` (DateTime): Timestamp of soft deletion.
