
# Email Sender API Documentation

## Overview

The Email Sender API is designed to allow users to manage email settings, send individual emails, and perform bulk email sending through a Django-based backend with Django REST Framework. Once configured, the user can send emails based on saved settings, which include host, port, protocol, and authentication details.

### Key Features
- **Email Settings Management**: Configure and save email settings (host, protocol, etc.).
- **Single and Bulk Email Sending**: Choose to send a single email or a bulk email to multiple recipients.
- **Settings Update**: Allows updating the email configuration settings at any time.

## Endpoints

### 1. Email Settings

**Endpoint:** `/api/email/settings/`

- **Method**: `POST`
- **Description**: Save or update email settings such as host, port, protocol, username, and password.
- **Body Parameters**:
  - `host` (string): The email server's host.
  - `port` (integer): The email server's port.
  - `protocol` (string): Protocol used (e.g., `smtp`).
  - `username` (string): Username for authentication.
  - `password` (string): Password for authentication.
  - `use_tls` (boolean): Enable TLS if `true`.
  - `use_ssl` (boolean): Enable SSL if `true`.

- **Response**: 
  - `201 Created` if the settings are saved successfully.
  - Returns the saved settings.

**Example Request**:
```json
{
  "host": "smtp.example.com",
  "port": 465,
  "protocol": "smtp",
  "username": "user@example.com",
  "password": "password",
  "use_tls": false,
  "use_ssl": true
}
```

**Example Response**:
```json
{
  "id": 1,
  "host": "smtp.example.com",
  "port": 465,
  "protocol": "smtp",
  "username": "user@example.com",
  "use_tls": false,
  "use_ssl": true
}
```

### 2. Send Single Email

**Endpoint:** `/api/email/send/`

- **Method**: `POST`
- **Description**: Send a single email to one recipient using the saved settings.
- **Body Parameters**:
  - `to_email` (string): Recipient's email address.
  - `subject` (string): Subject of the email.
  - `message` (string): Body content of the email.

- **Response**:
  - `200 OK` if the email is sent successfully.
  - Returns a success message.

**Example Request**:
```json
{
  "to_email": "recipient@example.com",
  "subject": "Hello",
  "message": "This is a test email."
}
```

**Example Response**:
```json
{
  "status": "Email sent successfully"
}
```

### 3. Send Bulk Email

**Endpoint:** `/api/email/send-bulk/`

- **Method**: `POST`
- **Description**: Send emails to multiple recipients in a single request using saved settings.
- **Body Parameters**:
  - `to_emails` (array of strings): List of recipient email addresses.
  - `subject` (string): Subject of the email.
  - `message` (string): Body content of the email.

- **Response**:
  - `200 OK` if all emails are sent successfully.
  - Returns a success message with details of sent emails.

**Example Request**:
```json
{
  "to_emails": ["recipient1@example.com", "recipient2@example.com"],
  "subject": "Hello All",
  "message": "This is a test email for bulk sending."
}
```

**Example Response**:
```json
{
  "status": "Bulk email sent successfully",
  "sent_count": 2
}
```

## Error Handling

- **400 Bad Request**: For missing or invalid fields.
- **500 Internal Server Error**: For any server-side issues, such as email configuration errors.

## Models

### EmailSettings
- `host` (CharField)
- `port` (IntegerField)
- `protocol` (CharField)
- `username` (CharField)
- `password` (CharField)
- `use_tls` (BooleanField)
- `use_ssl` (BooleanField)

### Sending Process

- **Single Email**: Calls the `/api/email/send/` endpoint, using the saved settings.
- **Bulk Email**: Calls the `/api/email/send-bulk/` endpoint, looping over recipients using saved settings.
