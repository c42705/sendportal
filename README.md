<p align="center"><img src="https://sendportal.io/img/sendportal.png" width="250"></p>


Modern open-source self-hosted email marketing.

- [Website](https://sendportal.io)
- [Documentation](https://sendportal.io/docs)

## Introduction

The core functionality of SendPortal is contained within the [SendPortal Core](https://github.com/mettle/sendportal-core) package. If you would like to add SendPortal to an existing application that already handles user authentication, you only require [SendPortal Core](https://github.com/mettle/sendportal-core).

## Features
SendPortal includes subscriber and list management, email campaigns, message tracking, reports and multiple workspaces/domains in a modern, flexible and scalable application.

SendPortal integrates with [Amazon SES](https://aws.amazon.com/ses), [Postmark](https://postmarkapp.com), [Sendgrid](https://sendgrid.com), [Mailgun](https://www.mailgun.com/) and [Mailjet](https://www.mailjet.com).

The [SendPortal](https://github.com/mettle/sendportal) application acts as a wrapper around SendPortal Core. This will allow you to run your own copy of SendPortal as a stand-alone application, including user authentication and multiple workspaces.

## Installation

If you would like to install SendPortal as a stand-alone application, please follow the [installation guide](https://sendportal.io/docs/v2/getting-started/installation).

If you would like to add SendPortal to an existing application, please follow the [package installation guide](https://sendportal.io/docs/v2/getting-started/package-installation).

## Requirements
SendPortal V3 requires:

- PHP 8.2+
- Laravel 10+
- MySQL (≥ 5.7) or PostgreSQL (≥ 9.4)

If you are on an earlier version of PHP (7.3+) or Laravel (8+), please use [SendPortal V2](https://github.com/mettle/sendportal/releases/tag/v2.0.4)

## Docker Deployment

SendPortal can be easily deployed using Docker and Docker Compose. This section explains how to set up and run SendPortal in a containerized environment.

### Prerequisites

- Docker and Docker Compose installed on your system
- Git for cloning the repository

### Docker Setup Files

The repository includes the following Docker configuration files:

- `Dockerfile`: Defines the PHP application container
- `docker-compose.yml`: Orchestrates the multi-container setup
- `nginx/conf.d/app.conf`: Nginx web server configuration
- `php/local.ini`: PHP configuration settings

### Deployment Steps

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/sendportal.git
   cd sendportal
   ```

2. **Set up environment variables**:
   ```bash
   cp .env.example .env
   ```
   
   Update the `.env` file with appropriate values, particularly:
   ```
   DB_CONNECTION=mysql
   DB_HOST=db
   DB_PORT=3306
   DB_DATABASE=sendportal
   DB_USERNAME=sendportal
   DB_PASSWORD=your_secure_password
   
   REDIS_HOST=redis
   REDIS_PASSWORD=null
   REDIS_PORT=6379
   
   QUEUE_CONNECTION=redis
   ```

3. **Build and start the containers**:
   ```bash
   docker-compose build
   docker-compose up -d
   ```

4. **Run the installation command**:
   ```bash
   docker-compose exec app php artisan sp:install
   ```
   
   This will guide you through setting up:
   - Application key
   - Application URL
   - Database connection
   - Database migrations
   - Admin user account

5. **Access the application**:
   Once installed, you can access SendPortal at `http://localhost:8080`

### Container Structure

- **app**: PHP-FPM container running the Laravel application
- **webserver**: Nginx web server
- **db**: MySQL database
- **redis**: Redis for caching and queues

### Persistent Data

Database data is stored in a Docker volume named `sendportal-data` to ensure persistence between container restarts.

### Customization

You can modify the Docker setup by editing:
- `docker-compose.yml` to change port mappings or add environment variables
- `Dockerfile` to customize the PHP environment
- `nginx/conf.d/app.conf` to adjust web server settings

### Production Considerations

For production deployment:
1. Use a proper domain name and configure SSL
2. Set `APP_ENV=production` and `APP_DEBUG=false` in your `.env` file
3. Consider using a managed database service instead of the containerized MySQL
4. Set up proper monitoring and logging
