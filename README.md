# VProfile - DevOps Sample Application

VProfile is a sample Java web application built with Spring MVC, demonstrating end-to-end DevOps practices including CI/CD, containerization, orchestration, and infrastructure as code. It features user management, integrates with MySQL (database), Memcached (caching), RabbitMQ (messaging), and Elasticsearch (search), and showcases deployment across multiple environments using tools like Docker, Kubernetes (via Helm), Ansible, and Terraform.

## Prerequisites

Before setting up or running the application, ensure you have the following installed:

- **JDK 11** (or later compatible version, as specified in [pom.xml](pom.xml))
- **Maven 3** (for building the project)
- **MySQL 8** (for the database; see database setup below)
- **Docker** (for containerized deployment)
- **kubectl** (for Kubernetes interactions, if deploying to K8s)
- **Helm** (for Kubernetes chart deployments)
- **Ansible** (for configuration management)
- **Terraform** (for infrastructure provisioning)
- **Git** (for cloning the repository)

Optional for full CI/CD:
- **SonarQube** (for code analysis)
- **Nexus** (for artifact management)
- **AWS CLI** (for cloud deployments via GitHub Actions)

## Technologies Used

- **Backend**: Spring MVC, Spring Security, Spring Data JPA
- **Frontend**: JSP (JavaServer Pages)
- **Database**: MySQL (configured in [src/main/resources/application.properties](src/main/resources/application.properties))
- **Caching**: Memcached
- **Messaging**: RabbitMQ
- **Search**: Elasticsearch
- **Build Tool**: Maven
- **Server**: Apache Tomcat (configured via [files/context.xml](files/context.xml) and [files/tomcat-users.xml](files/tomcat-users.xml))
- **Containerization**: Docker (see [Dockerfile](Dockerfile))
- **Orchestration**: Kubernetes (Helm charts in [kubernetes/](kubernetes/))
- **Configuration Management**: Ansible (playbooks in [ansible/](ansible/))
- **Infrastructure as Code**: Terraform (scripts in [terraform/](terraform/))
- **CI/CD**: GitHub Actions (workflows in [.github/workflows/](.github/workflows/))

## Database Setup

1. Install MySQL 8 on your system (e.g., on Ubuntu: `sudo apt-get update && sudo apt-get install mysql-server`).
2. Create a database named `accounts`.
3. Import the database schema from [src/main/resources/db_backup.sql](src/main/resources/db_backup.sql) (assuming it exists; if not, create tables for user accounts based on the app's JPA entities).
   ```bash
   mysql -u <username> -p accounts < src/main/resources/db_backup.sql
   ```
4. Update database credentials in [src/main/resources/application.properties](src/main/resources/application.properties) or [ansible/templates/application.j2](ansible/templates/application.j2) for your environment.

## Installation and Setup

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/utkarshpopli26/vprofile-action.git
   cd vprofile-action
   ```

2. Build the project with Maven:
   ```bash
   mvn clean install
   ```

3. Configure the application properties (e.g., update DB host, ports in [src/main/resources/application.properties](src/main/resources/application.properties)).

4. Run the application locally (requires Tomcat):
   - Deploy the WAR file (`target/vprofile-v2.war`) to Tomcat's `webapps` directory.
   - Start Tomcat: `catalina.sh run`.
   - Access the app at `http://localhost:8080`.

### Docker Deployment

1. Build and run the Docker container:
   ```bash
   docker build -t vprofile .
   docker run -p 8080:8080 vprofile
   ```
   - The [Dockerfile](Dockerfile) uses a multi-stage build: compiles with Maven and deploys to Tomcat.

2. For full stack, ensure dependent services (MySQL, Memcached, etc.) are running (e.g., via Docker Compose if available).

### Kubernetes Deployment

1. Apply the Kubernetes manifests from [kubernetes/](kubernetes/):
   ```bash
   kubectl apply -f kubernetes/vpro-app/
   ```
   - This includes deployments, services, ingress, and secrets for the app, DB, Memcached, RabbitMQ, etc.

2. Access via the configured ingress (e.g., via LoadBalancer or Ingress controller).

### Ansible Provisioning

Run Ansible playbooks to set up environments (e.g., Tomcat, app config):
```bash
ansible-playbook -i inventory ansible/site.yml
```
- Templates like [ansible/templates/application.j2](ansible/templates/application.j2) configure app properties.

### Terraform Infrastructure

Provision cloud resources (e.g., EKS cluster):
```bash
cd terraform
terraform init
terraform apply
```
- See [terraform/main.tf](terraform/main.tf) for AWS EKS setup.

## Usage

- **Web Interface**: After deployment, visit the app URL (e.g., `http://localhost:8080` or your K8s ingress). The home page ([src/main/webapp/WEB-INF/views/index_home.jsp](src/main/webapp/WEB-INF/views/index_home.jsp)) showcases technologies; the welcome page ([src/main/webapp/WEB-INF/views/welcome.jsp](src/main/webapp/WEB-INF/views/welcome.jsp)) includes user posts and comments.
- **API/Features**: Supports user accounts, search via Elasticsearch, caching with Memcached, and messaging via RabbitMQ.
- **CI/CD**: Push to the repo to trigger GitHub Actions pipelines for automated build, test, and deploy. See [.github/workflows/main.yml](.github/workflows/main.yml) for app CI/CD and [.github/workflows/terraform.yml](.github/workflows/terraform.yml) for infrastructure changes.

## Testing

- Run unit tests: `mvn test`
- Integration tests are configured in the GitHub Actions workflows with SonarQube analysis.

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit changes and push.
4. Open a pull request.

## License

This project is licensed under the Apache License 2.0 (see [files/context.xml](files/context.xml) and [files/tomcat-users.xml](files/tomcat-users.xml) for Apache references).

## Support

For issues, check the GitHub Actions logs or open a GitHub issue. Ensure all prerequisites are met before reporting bugs.
