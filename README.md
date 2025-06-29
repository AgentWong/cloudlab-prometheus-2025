# Prometheus Monitoring Stack with Ansible Molecule

A complete monitoring infrastructure deployment using Ansible with comprehensive Molecule testing. This project demonstrates enterprise-ready automation practices by deploying a Prometheus-based monitoring stack across containerized environments with full functional validation.

## Architecture

The implementation deploys a two-node monitoring architecture using established Ansible collections:

**Monitoring Server Components:**
- Prometheus for metrics collection and storage
- Grafana for visualization and dashboard management
- AlertManager for notification handling
- Node Exporter for system metrics

**Database Server Components:**
- PostgreSQL with sample test data
- Postgres Exporter for database metrics
- Node Exporter for system monitoring

**Cross-Server Integration:**
- Prometheus scrapes metrics from both servers
- Grafana connects to Prometheus as a datasource
- Alert rules monitor both system and application metrics
- Automated dashboard provisioning with PostgreSQL and Node Exporter dashboards

## Requirements

**System Dependencies:**
- Docker for containerized testing environments
- Python 3.13 or compatible version
- Ansible Core for automation execution

**Python Dependencies:**
```
ansible-compat
molecule
molecule-plugins[docker]
```

**Ansible Collections:**
- prometheus.prometheus for monitoring components
- community.general for utility modules
- community.grafana for dashboard automation
- community.postgresql for database management
- ansible.posix for system operations

**Additional Roles:**
- geerlingguy.postgresql for PostgreSQL installation

## Installation

**Install Python dependencies:**
```bash
pip install -r molecule/requirements.txt
```

**Install Ansible collections and roles:**
```bash
ansible-galaxy install -r molecule/default/requirements.yml
```

## Usage

**Run complete test suite:**
```bash
molecule test
```

**Deploy stack for development:**
```bash
molecule converge
```

**Run verification tests only:**
```bash
molecule verify
```

**Multi-distribution testing:**
```bash
# Ubuntu 22.04 (default)
DOCKER_IMAGE=geerlingguy/docker-ubuntu2204-ansible DOCKER_TAG=latest molecule test

# Debian 11
DOCKER_IMAGE=geerlingguy/docker-debian11-ansible DOCKER_TAG=latest molecule test
```

## Testing Framework

The project implements comprehensive functional testing that validates actual application behavior rather than simple configuration compliance. The verification process includes:

**Service Validation:**
- Confirms all services are running and responsive
- Tests API endpoints for operational functionality
- Validates cross-service communication patterns

**Metrics Collection Verification:**
- Queries Prometheus API to confirm metrics ingestion
- Tests PostgreSQL metrics through postgres_exporter
- Validates node metrics from both servers

**Database Integration Testing:**
- Confirms PostgreSQL connectivity and sample data availability
- Tests postgres_exporter authentication and permissions
- Validates database-specific metrics collection

**Dashboard and Alerting Verification:**
- Confirms Grafana health and datasource connectivity
- Validates alert rule configuration and loading
- Tests dashboard provisioning functionality

## Configuration

The stack uses environment-specific variables for different deployment contexts. Testing configurations include vault passwords defined in the molecule.yml inventory, while production deployments should use proper Ansible Vault encryption.

**Key Configuration Areas:**
- PostgreSQL databases and user management
- Prometheus scrape target configuration
- Grafana datasource and dashboard provisioning
- Alert rule definitions and routing

## GitHub Actions Integration

The project includes automated testing through GitHub Actions with matrix testing across multiple Linux distributions. The workflow validates compatibility and functionality across Ubuntu 22.04 and Debian 11 environments, providing confidence for heterogeneous infrastructure deployments.

**Workflow Features:**
- Parallel testing across multiple distributions
- Dependency caching for improved performance
- Comprehensive timeout handling for reliable execution
- Detailed logging and error reporting

## Project Structure

```
.
├── ansible.cfg                  # Ansible configuration settings
├── LICENSE                     # Project license
├── molecule/
│   ├── default/
│   │   ├── converge/           # Deployment task modules
│   │   │   ├── grafana-config.yml
│   │   │   ├── grafana-setup.yml
│   │   │   ├── postgres-activity.yml
│   │   │   ├── postgres-config.yml
│   │   │   └── postgres-setup.yml
│   │   ├── converge.yml        # Main deployment playbook
│   │   ├── group_vars/         # Ansible group variables
│   │   ├── molecule.yml        # Molecule configuration and inventory
│   │   ├── prepare.yml         # Container preparation tasks
│   │   ├── README.md          # Scenario-specific documentation
│   │   ├── requirements.yml    # Collection and role dependencies
│   │   ├── verify/            # Individual verification tasks
│   │   │   ├── alertmanager-check.yml
│   │   │   ├── grafana-check.yml
│   │   │   ├── integration-check.yml
│   │   │   ├── node-exporter-check.yml
│   │   │   ├── postgres-check.yml
│   │   │   ├── postgres-exporter-check.yml
│   │   │   ├── prometheus-check.yml
│   │   │   └── summary-display.yml
│   │   └── verify.yml          # Test orchestration
│   └── requirements.txt        # Python dependencies
├── prep_host.yml              # Host preparation playbook
├── README.md                  # Project documentation
└── roles/                     # Custom Ansible roles directory
```

## Development Benefits

This implementation demonstrates the transformative impact of containerized testing on infrastructure automation development. The complete test cycle executes in approximately six minutes, representing a dramatic improvement over traditional testing approaches that typically require thirty to sixty minutes for cloud provisioning and cleanup.

**Key Advantages:**
- Rapid iteration cycles enable experimental development
- Multi-distribution validation catches compatibility issues early
- Functional testing validates end-to-end application behavior
- Automated CI/CD integration provides continuous validation

## Implementation Notes

The project addresses common containerization challenges including DNS resolution between services, certificate validation, and resource constraints. The roles accommodate both containerized testing and production deployment patterns through conditional logic and environment-specific configuration.

**Container Networking:**
- Custom Docker network enables cross-container communication
- Published ports provide external access to monitoring interfaces
- Service discovery uses container hostnames for internal connectivity

**Database Integration:**
- PostgreSQL includes sample data generation for realistic testing
- Postgres_exporter configuration handles authentication and permissions
- Database activity simulation provides meaningful metrics for validation

## Accessing Services

When running with molecule converge, services are accessible on the following ports:

- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000 (admin/grafanapass123)
- AlertManager: http://localhost:9093
- PostgreSQL: localhost:5432
- Postgres Exporter: http://localhost:9187/metrics

## License

This project serves as a demonstration of modern Ansible automation practices with comprehensive testing frameworks. The implementation showcases enterprise-ready patterns for infrastructure monitoring deployment and validation.