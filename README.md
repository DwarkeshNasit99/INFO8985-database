# LCBO Database Monitoring with OpenTelemetry Collector

This project demonstrates a full monitoring setup for a PostgreSQL database using the OpenTelemetry Collector, including both built-in PostgreSQL metrics and a custom SQL query metric. The stack runs entirely in Docker Compose and includes pgAdmin for database management.

## Stack Overview

- **PostgreSQL**: The main database service (`demodb`).
- **pgAdmin**: Web UI for managing PostgreSQL.
- **OpenTelemetry Collector**: Collects metrics from PostgreSQL and runs custom SQL queries, exposing them in Prometheus format.

## How to Run

1. **Start the stack:**
   docker-compose -f lcbo/docker-compose.yml up -d
 
   This will start PostgreSQL, pgAdmin, and the OpenTelemetry Collector.

2. **Access pgAdmin:**
   - Open [http://localhost:5050](http://localhost:5050)
   - Login:
     - Email: `devteds@pgadmin.org`
     - Password: `devteds`
   - Add a new server with:
     - Host: `db`
     - Port: `5432`
     - Username: `devtedsuser`
     - Password: `devtedspass`

3. **Database Setup:**
   - The database `demodb` is created automatically by Docker Compose.

## Metrics Collected

### 1. PostgreSQL Built-in Metrics
The OpenTelemetry Collector collects a wide range of PostgreSQL metrics, such as:
- Number of backends
- Database size
- Table and index statistics
- Commits, rollbacks, and more

### 2. Custom SQL Query Metric
A custom metric is collected using the following query:

SELECT count(*) as total_products FROM products;

This is exposed as a Prometheus metric:

lcbo_products_total_ratio <number_of_products>

## Accessing Metrics

- OpenTelemetry Collector exposes metrics at: [http://localhost:8889/metrics](http://localhost:8889/metrics)
- You will see both PostgreSQL and custom metrics in Prometheus format.

## Example Output

# HELP lcbo_products_total_ratio Total number of products in the database
# TYPE lcbo_products_total_ratio gauge
lcbo_products_total_ratio 1