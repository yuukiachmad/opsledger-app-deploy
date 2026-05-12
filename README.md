# OpsLedger Application Deployment

OpsLedger is an internal operations ledger dashboard used to demonstrate a complete application delivery workflow on AWS EKS.

The project includes a Java web application, MySQL, Memcached, RabbitMQ, SonarCloud quality analysis, Docker image publishing to Amazon ECR, Helm deployment to EKS, and GitHub Actions CI/CD.

## Stack

- Java 11
- Maven
- Spring MVC and JSP
- MySQL
- Memcached
- RabbitMQ
- Docker
- SonarCloud
- Helm
- Kubernetes on Amazon EKS
- GitHub Actions

## Local Build

```bash
mvn test
mvn package
```

The build produces `target/opsledger-v1.war`.

For local runtime testing, provide the application credentials as environment variables:

```bash
export OPSLEDGER_DB_USERNAME=root
export OPSLEDGER_DB_PASSWORD=<database-password>
export OPSLEDGER_RABBITMQ_USERNAME=<rabbitmq-username>
export OPSLEDGER_RABBITMQ_PASSWORD=<rabbitmq-password>
```

## Container Build

```bash
docker build -t opsledger-web:local .
```

## Helm Deployment

```bash
kubectl create secret generic opsledger-secret \
  --from-literal=db-pass=<database-password> \
  --from-literal=rmq-user=<rabbitmq-username> \
  --from-literal=rmq-pass=<rabbitmq-password>

helm upgrade --install opsledger-stack helm/opsledger \
  --set appimage=<account-id>.dkr.ecr.ap-southeast-3.amazonaws.com/opsledger-web \
  --set apptag=<image-tag>
```

## Required GitHub Secrets

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `REGISTRY`
- `SONAR_TOKEN`

## Required GitHub Variables

- `SONAR_ORGANIZATION`
- `SONAR_PROJECT_KEY`

`REGISTRY` should use the ECR registry hostname, for example:

```text
<account-id>.dkr.ecr.ap-southeast-3.amazonaws.com
```

The `opsledger-infra` project creates the `opsledger-web` ECR repository. The GitHub Actions workflow is manual-only by default so the public repository can be published safely before enabling automatic branch triggers. Run the workflow manually with `publish=true` when the repository secrets, variables, ECR repository, EKS cluster, and Kubernetes `opsledger-secret` are ready.

## Runtime Components

- `opsledger-web`: Java web application.
- `opsledger-db`: MySQL database.
- `opsledger-cache`: Memcached service for repeated account lookups.
- `opsledger-queue`: RabbitMQ service for approval queue events.
- `opsledger-ingress`: ingress route to the web service.

## Documentation

- `docs/architecture.md`: application deployment architecture.
- `docs/deployment-guide.md`: deployment walkthrough and validation checklist.
- `docs/screenshots/`: final app and platform screenshots.
