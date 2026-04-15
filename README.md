# OwnCloud Deployment with Docker-Compose

## Overview
This project provides a Docker-Compose setup for deploying OwnCloud along with MariaDB, Redis, and Collabora on the Coolify Platform. In the coolify setup, we have taken the approach to have a control node for the Coolify admin ui. A second worker node is deployed for the docker workloads. The expectation is that more workers could be added, if and when needed.

## Components
- **OwnCloud**: A self-hosted file sync and share server.
- **MariaDB**: A relational database management system, a drop-in replacement for MySQL.
- **Redis**: An in-memory data structure store, used as a database, cache, and message broker.
- **Collabora**: An online office suite based on LibreOffice, providing document editing capabilities.

## Prerequisites
- Docker installed on your system.
- Docker Compose installed on your system.

## Getting Started
1. **Clone the Repository**
   ```bash
   git clone https://github.com/jsal-gh/owncloud.git
   cd owncloud
   ```

2. **Configure Environment Variables**
   Create a `.env` file in the root of the project and define the necessary environment variables for your applications.
   
3. **Start the Services**
   Run the following command to start all services:
   ```bash
   docker-compose up -d
   ```

4. **Access OwnCloud**
   Open your browser and go to `http://localhost:8080` to access your OwnCloud instance.

## Stopping the Services
To stop the running services, use the command:
```bash
docker-compose down
```

## Usage
Refer to the official documentation of OwnCloud, MariaDB, Redis, and Collabora for further configuration and usage instructions.

## Contributing
If you'd like to contribute to this project, please fork the repository and submit a pull request.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

**Last Updated**: 2026-04-15 02:38:02 (UTC)
