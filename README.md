# Revlogica Microservice Platform

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-in%20development-orange.svg)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

## About The Project

Revlogica is a modular, microservice-based platform designed for the digital humanities. Its goal is to enable researchers to ingest, analyze, validate, and enrich cultural heritage manuscript data, producing structured, linked knowledge graphs.

This repository serves as the root for the entire Revlogica project, containing all the necessary services and configurations to run the platform using Docker.

## Architecture

The platform is composed of several independent microservices that work together.

- [**revlogica-orchestrator**](./revlogica-orchestrator/): The central FastAPI orchestrator for the RevLogica project. It supports the ingestion of XML/TEI manuscript files and oversees their passage through the pipeline: archiving in eXist-DB, NLP analysis via a dedicated microservice, semantic linking in Fuseki, and full-text indexing in Elasticsearch. The service is built following the principles of **Clean Architecture** to ensure separation of concerns and maintainability.

- [**revlogica-item-authoring**](./revlogica-item-authoring/): An Omeka-S based application enabling researchers to author manuscript items. It provides tools for transcribing historical texts, linking IIIF resources, and creating structured metadata.

- [**revlogica-nlp-microservice**](./revlogica-nlp-microservice/): A FastAPI-based microservice providing Natural Language Processing capabilities. Built on spaCy, it performs Named Entity Recognition (NER) to extract people, places, and concepts.

- [**revlogica-data-validation**](./revlogica-data-validation/): A Django-based application enabling researchers to review and validate NLP-generated results. This Human-in-the-Loop step ensures only expert-approved data is integrated into the knowledge graph.

## Getting Started

Follow these instructions to get the entire Revlogica platform running on your local machine for development.

### Prerequisites

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/products/docker-desktop) & [Docker Compose](https://docs.docker.com/compose/)

### Installation

1.  **Clone the Root Project:**
    This repository contains the main `docker-compose.yml` file and environment configuration.

    ```bash
    git clone https://github.com/ricelq/revlogica-project-root.git
    cd revlogica-project-root
    ```

2.  **Clone the Microservice Repositories:**
    From within the `revlogica-project-root` directory, clone each of the required microservices. The `docker-compose.yml` file expects them to be in this structure.

    ```bash
    git clone https://github.com/ricelq/revlogica-orchestrator.git
    git clone https://github.com/ricelq/revlogica-item-authoring.git
    git clone https://github.com/ricelq/revlogica-nlp-microservice.git
    git clone https://github.com/ricelq/revlogica-data-validation.git
    ```

3.  **Create Your Environment File:**
    Copy the example environment file and fill in your specific configuration details for all services.

    ```bash
    cp .env.example .env
    ```

    Now, edit the `.env` file with your credentials and settings.

4.  **Build and Run the Containers:**
    This command will build the images and start all the services defined in the `docker-compose.yml` file.

    ```bash
    docker-compose up -d --build
    ```

    The initial build may take some time as it needs to download all the base images and install dependencies for each service.

5.  **Service-Specific Setup:**
    Some services may require additional setup steps, such as database migrations or creating an admin user. Please refer to the `README.md` file within each service's directory for specific instructions.

    - Example for `revlogica-data-validation`:
      ```bash
      # Apply database migrations
      docker-compose exec validation-app python manage.py migrate
      # Create a superuser
      docker-compose exec validation-app python manage.py createsuperuser
      ```

## Usage

Once the containers are running, the different services will be accessible at their respective ports. Please refer to the `docker-compose.yml` file and the individual service documentation for specific URLs and ports.

To interact with the main orchestrator API, you can use the interactive Swagger documentation available at:

- **Orchestrator API (Swagger UI):** [http://localhost:8000/doc](http://localhost:8000/doc)

This interface allows you to view all available endpoints, test them directly from your browser, and see the expected request/response models.

## License

This project is licensed under the MIT License.

## Contact

Ricel Quispe - [@linkedin](https://www.linkedin.com/in/ricelquispe) - ricel@prodeimat.ch

Project Link: [https://github.com/ricelq/revlogica-project-root](https://github.com/ricelq/revlogica-project-root)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
