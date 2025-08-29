# Backend Specification: Contact Form Mailer

## Overview
A minimal backend service to securely send emails from a contact form. The service should be simple to deploy, maintain, and secure against common threats (spam, abuse, injection, etc.).

## Requirements

### Functional
- Expose a single HTTP endpoint (e.g., `/contact`) to receive contact form submissions (name, email, message).
- Validate and sanitize all incoming data.
- Send the form data as an email to a configured recipient address.
- Return a success or error response to the client.

### Security
- Rate limit requests to prevent spam/abuse (e.g., per IP, per minute).
- Use environment variables for sensitive configuration (SMTP credentials, recipient email, etc.).
- Sanitize all inputs to prevent injection attacks.
- Support CORS configuration to restrict allowed origins.
- Optionally support CAPTCHA verification (e.g., reCAPTCHA) for extra protection.

### Deployment
- Should run as a simple Node.js/Express (or similar) service.
- Containerize with Docker for easy deployment.
- Provide a sample `docker-compose.yml` for local development.
- Document environment variables and setup steps in a `README.md`.

### Non-Functional
- Minimal dependencies, fast startup.
- Clear error handling and logging (avoid leaking sensitive info).
- Codebase should be easy to read and maintain.

## Example Request
```json
POST /contact
{
  "name": "John Doe",
  "email": "john@example.com",
  "message": "Hello, I have a question."
}
```

## Example Response
```json
{
  "success": true,
  "message": "Your message has been sent."
}
```

## Out of Scope
- No user authentication or database storage required.
- No frontend/UI implementation.
