# Task: Configure Application Logging Stack

## 1. Objective
Configure containerized application logs that are exposed to standard Docker commands, and persisted to the host machine via Docker volume mounts, and rotated based on time and file size.

## 2. Requirements

### Language & Framework Selection
* Detect the core language of the project and choose a standard, industry-recognized logging framework.
* Implement a central logger instance (e.g., `src/utils/logger.js` or `src/utils/logger.py`) used across all application modules.

### Container Stream Routing (`docker logs`)
* Route all `INFO` and `DEBUG` log events to `stdout`.
* Route all `WARN`, `ERROR`, and `FATAL` log events to `stderr`.
* Use structured formatting (JSON in production, colorized text in local development).

### File Persistence & Rotation
* Target Log Directory: Environment variable `LOG_DIR`, falling back to `./data/logs`.
* Output Files:
  * Standard Log File: `app-%DATE%.log` (contains `INFO` level and above).
  * Error Log File: `error-%DATE%.log` (contains `ERROR` level and above).
* Rotation Policy:
  * Size Trigger: Rotate files when they exceed `10MB` or `20MB`.
  * Daily Trigger: Rotate files every day at midnight (`00:00`).
  * Retention Limit: Retain general log files for 14 days and error log files for 30 days.
  * Compression: Compress rotated log files (`.gz` or `.zip`).

### Docker & Host Mount Configuration
* Update `docker-compose.yml` to create a volume mount mapping on the host to `/app/data/logs` in the container. Host location will be inside the application's data folder, which should be a volume mount found in the Docker Compose file, at a location like `/pwspool/software/applicationname` or similar. Create a `logs/` folder in there and mount that.
* Add Docker logging driver constraints under the application service in `docker-compose.yml`:
  ```yaml
  logging:
    driver: "json-file"
    options:
      max-size: "10m"
      max-file: "3"
