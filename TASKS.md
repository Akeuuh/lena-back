# Development Tasks for Contact Form Mailer Backend

## Preliminary Setup

### Task 1: Project Initialization
**Prompt for Cursor:**
```
Initialize a TypeScript Node.js project with the following structure:
- Create package.json with scripts for dev, build, start
- Set up TypeScript configuration (tsconfig.json) with strict mode, ES2020 target
- Create folder structure: src/, src/routes/, src/services/, src/middleware/, src/utils/, logs/
- Install dependencies: express, nodemailer, yup, helmet, cors, express-rate-limit, winston, dotenv
- Install dev dependencies: @types/node, @types/express, typescript, ts-node, nodemon
- Create .env.example file with required environment variables
- Set up .gitignore to exclude node_modules, .env, logs/, dist/
- Create basic Express server in src/index.ts with middleware setup (helmet, cors, express.json)
- Add AXEL prefix to all console.log statements as per user preference
```

## Core Features

### Task 2: Contact Form Endpoint
**Prompt for Cursor:**
```
Create a POST /contact endpoint with the following requirements:
- Create src/routes/contact.ts with Express router
- Implement Yup validation schema for: name (string, required, min 2 chars), email (string, required, valid email), message (string, required, min 10 chars, max 1000 chars)
- Create src/controllers/contactController.ts to handle the business logic
- Sanitize all inputs to prevent XSS attacks
- Return JSON responses: success {success: true, message: "Your message has been sent"} or error {success: false, message: "Error description"}
- Add proper error handling with try-catch blocks
- Integrate with logging service (use console.log with AXEL prefix for now)
```

### Task 3: Email Service Implementation
**Prompt for Cursor:**
```
Create email service using Nodemailer with Brevo SMTP:
- Create src/services/emailService.ts
- Configure Nodemailer transporter for Brevo SMTP (smtp-relay.brevo.com, port 587, STARTTLS)
- Use environment variables: BREVO_SMTP_USER, BREVO_SMTP_PASS, RECIPIENT_EMAIL, FROM_EMAIL
- Create sendContactEmail function that accepts {name, email, message} and formats email with:
  - Subject: "New Contact Form Submission"
  - HTML template with sender details and message
  - Plain text fallback
- Add comprehensive error handling with specific error messages
- Add AXEL prefixed logging for successful sends and errors
```

### Task 4: Security Middleware
**Prompt for Cursor:**
```
Implement security measures:
- Create src/middleware/rateLimiter.ts using express-rate-limit
- Set rate limit to 5 requests per minute per IP for /contact endpoint
- Add custom rate limit exceeded message
- Create src/middleware/security.ts with input sanitization functions
- Configure helmet middleware for security headers
- Set up CORS with specific allowed origins (use environment variable ALLOWED_ORIGINS)
- Add request logging middleware that logs IP, method, URL with AXEL prefix
```

### Task 5: Logging System
**Prompt for Cursor:**
```
Set up Winston logging system:
- Create src/utils/logger.ts using Winston
- Configure multiple transports: console (all levels) and file (error level to logs/error.log, info level to logs/app.log)
- Use JSON format with timestamps
- Create log rotation to prevent large files
- Replace all console.log statements with proper logger calls, keeping AXEL prefix in the message content
- Add logging for: incoming requests, validation errors, email sending success/failure, rate limit hits
```

### Task 6: Environment Configuration
**Prompt for Cursor:**
```
Set up comprehensive environment configuration:
- Create src/config/index.ts to centralize all environment variables
- Define interfaces for configuration with proper types
- Add validation for required environment variables on startup
- Create detailed .env.example with all required variables and descriptions:
  - PORT, NODE_ENV
  - BREVO_SMTP_USER, BREVO_SMTP_PASS
  - RECIPIENT_EMAIL, FROM_EMAIL
  - ALLOWED_ORIGINS
  - RATE_LIMIT_WINDOW_MS, RATE_LIMIT_MAX_REQUESTS
- Add startup validation that exits process if required env vars are missing
```

## Deployment Preparation

### Task 7: Docker Configuration
**Prompt for Cursor:**
```
Create Docker deployment setup:
- Create Dockerfile with multi-stage build (build stage and production stage)
- Use node:18-alpine as base image for production
- Create .dockerignore file
- Create docker-compose.yml for local development with:
  - App service with volume mounts for development
  - Environment variables setup
  - Port mapping
  - Restart policies
- Add npm scripts for Docker operations (docker:build, docker:run, docker:dev)
```

### Task 8: Production Readiness
**Prompt for Cursor:**
```
Prepare application for VPS deployment:
- Create src/middleware/healthCheck.ts with /health endpoint returning server status
- Add graceful shutdown handling in src/index.ts (SIGTERM, SIGINT)
- Create production build process that compiles TypeScript to dist/
- Add process.env.NODE_ENV checks for production vs development behavior
- Create startup script that ensures logs directory exists
- Add error handling for uncaught exceptions and unhandled rejections
- Configure production logging to rotate files and limit size
```

## Optional Features

### Task 9: reCAPTCHA Integration (Optional)
**Prompt for Cursor:**
```
Add Google reCAPTCHA v2 verification:
- Install google-recaptcha2 package
- Create src/services/recaptchaService.ts
- Add RECAPTCHA_SECRET_KEY to environment configuration
- Modify contact form validation to include recaptcha token
- Add reCAPTCHA verification before processing form submission
- Add proper error handling for reCAPTCHA failures
- Make reCAPTCHA optional via environment variable ENABLE_RECAPTCHA
```

### Task 10: API Documentation
**Prompt for Cursor:**
```
Create comprehensive documentation:
- Update README.md with:
  - Project description and features
  - Installation and setup instructions
  - Environment variables documentation
  - API endpoint documentation
  - Docker deployment instructions
  - VPS deployment guide
- Create API.md with detailed endpoint specifications
- Include example requests and responses
- Add troubleshooting section
```

## Testing (Optional)

### Task 11: Unit Tests
**Prompt for Cursor:**
```
Set up Jest testing framework:
- Install jest, @types/jest, supertest, @types/supertest
- Create jest.config.js with TypeScript support
- Create tests/unit/ directory structure
- Write unit tests for:
  - Email service functionality
  - Input validation schemas
  - Rate limiting middleware
  - Contact controller logic
- Add npm scripts for running tests
- Mock external dependencies (nodemailer, recaptcha)
```

## Final Integration

### Task 12: Final Integration and Testing
**Prompt for Cursor:**
```
Complete final integration:
- Integrate all middleware into main Express app
- Set up proper error handling middleware as the last middleware
- Test all endpoints manually
- Verify environment variable loading
- Test email sending functionality
- Verify rate limiting works
- Check logging output in both console and files
- Ensure Docker build and run works correctly
- Create final deployment checklist in DEPLOYMENT.md
```
