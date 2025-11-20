# README.md 

The organization repository contains the docker-compose to start the application.

## Architecture

The system consists of four repositories:

- app https://github.com/doda2025-team22/app: The app contains the frontend of the application with REST API.
- model-service https://github.com/doda2025-team22/model-service: Backend for machine learning model, exposing port 8081 used by the frontend.
- operation https://github.com/doda2025-team22/operation: For deployment, orchestration and documentation.
- lib-version https://github.com/doda2025-team22/lib-version: Defines versioning for repositories.


## Steps to run the application:

First, you need docker and docker compose installed.

Start docker engine (open Docker Desktop for Windows for example)

Than you run the following commands:

`git clone git@github.com:doda2025-team22/operation.git`

`cd operation`

`docker compose up`

Open http://localhost:8080/sms
