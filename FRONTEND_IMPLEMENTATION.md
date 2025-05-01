# Feature Service API Implementation Guide for Frontend Developers

## Overview

This document provides implementation guidelines for frontend developers to integrate with the FeatureGuard Feature
Service APIs. The service provides endpoints to:

1. Retrieve application configuration with feature availability
2. Fetch user-specific feature access levels

## API Endpoints

### 1. Get Application Configuration

**Endpoint:** `GET /feature/app/{appId}/config`

**Description:** Retrieves the configuration for a specific application, including available feature types and their
enabled status.

**Parameters:**

- `appId` (path parameter): UUID of the application

**Response:**

```json
{
  "appId": "abb10bda-36a3-4e95-99b3-5ee92d75a4ee",
  "appName": "Terminal (AHC)",
  "featureTypes": [
    {
      "type": "login",
      "features": [
        {
          "name": "face",
          "enabled": true
        },
        {
          "name": "fingerprint",
          "enabled": false
        },
        {
          "name": "phone",
          "enabled": true
        },
        {
          "name": "email",
          "enabled": false
        }
      ]
    },
    {
      "type": "registration",
      "features": [
        {
          "name": "face",
          "enabled": true
        },
        {
          "name": "fingerprint",
          "enabled": false
        },
        {
          "name": "phone",
          "enabled": true
        },
        {
          "name": "email",
          "enabled": false
        }
      ]
    }
  ]
}
```

### 2. Get User-Specific Features

#### Production Endpoint (Requires Authentication)

**Endpoint:** `GET /feature/app/{appId}/features`

**Description:** Retrieves the features available to the authenticated user on a specific device, including access levels for
each feature. This endpoint requires JWT authentication.

**Parameters:**

- `appId` (path parameter): UUID of the application
- `deviceSerialNumber` (query parameter, optional for other apps apart from AHC): Serial number of the device
- `companyId` (query parameter, optional (only for admin app required)): UUID of the company

**Authentication:**
- Requires a valid JWT token in the request header

#### Testing Endpoint (No Authentication Required)

**Endpoint:** `GET /feature/app/{appId}/user-features`

**Description:** For testing purposes only. Retrieves the features available to a specific user on a specific device, including access levels for
each feature.

**Parameters:**

- `appId` (path parameter): UUID of the application
- `userId` (query parameter): UUID of the user
- `deviceSerialNumber` (query parameter, optional): Serial number of the device
- `companyId` (query parameter, optional (only for admin app required)): UUID of the company

**Response:**

```json
{
  "deviceId": "cd5e7a27-c53f-4b19-9366-760f96fc4c01",
  "facilityId": "04653b55-b3fd-4722-b9f3-292b59951512",
  "features": [
    {
      "featureType": "measurement",
      "features": [
        {
          "featureName": "measurementSummary",
          "accessLevel": [
            "read"
          ]
        },
        {
          "featureName": "spo2",
          "accessLevel": [
            "write"
          ]
        },
        {
          "featureName": "latestVitalSummary",
          "accessLevel": [
            "read"
          ]
        },
        {
          "featureName": "bloodPressure",
          "accessLevel": [
            "write"
          ]
        },
        {
          "featureName": "post_measurement_questionnaire",
          "accessLevel": [
            "read",
            "write"
          ]
        },
        {
          "featureName": "weight",
          "accessLevel": [
            "write"
          ]
        },
        {
          "featureName": "ecg",
          "accessLevel": [
            "write"
          ]
        },
        {
          "featureName": "questionnaire",
          "accessLevel": [
            "read",
            "write"
          ]
        },
        {
          "featureName": "streakSummary",
          "accessLevel": [
            "read"
          ]
        },
        {
          "featureName": "temperature",
          "accessLevel": [
            "write"
          ]
        }
      ]
    }
  ]
}
```

## Troubleshooting

### Common Issues

1. **403 Forbidden**: Check that you're using the correct appId and that the user has the necessary permissions
2. **404 Not Found**: Verify the appId is correct and the application exists
3. **Empty features array**: The user might not have any assigned features or roles
