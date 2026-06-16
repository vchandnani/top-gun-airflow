# Top Gun Airflow

Technology Playground: Apache Airflow, Docker, and AWS.

## References

https://docs.docker.com/get-started/

https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html

https://medium.com/better-programming/apache-airflow-on-docker-with-aws-s3-3abaf6874a49

## High-Level Design

1. Install prerequisites.
2. Configure AWS.
3. Clone the repository.
4. Docker configuration for Airflow.
5. Docker configuration for Airflow’s extended image.
6. Docker configuration for AWS.
7. Executing docker image to create containers.
8. DAG and Airflow Tasks creation.
9. Executing DAGs from Airflow UI.
10. Accessing S3 bucket/objects using AWS CLI.

## Prerequisites

Required:
- `Python` (validated with Python `3.14.0`)

Recommended:
- `git` latest stable

## AWS Configuration

1. Root User
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html
2. IAM User
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html
3. Access Keys
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html
4. S3 Buckets
https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html

## Clone Repository

```bash
git clone https://github.com/vchandnani/top-gun-airflow.git
cd top-gun-airflow
python3 -m venv .venv
source .venv/bin/activate
```

## Project Structure (3 Levels)
```text
.
├── config
│   └── airflow.cfg
├── dags
│   ├── __pycache__
│   │   ├── my_first_dag.cpython-313.pyc
│   │   └── weblog_gen.cpython-313.pyc
│   ├── my_first_dag.py
│   └── weblog_gen.py
├── docker-compose.yaml
├── Dockerfile
├── logs
│   ├── dag_id=my_first_dag
│   │   └── run_id=manual__2026-06-15T19:32:28.924791+00:00
│   ├── dag_processor
│   │   ├── 2026-06-04
│   │   ├── 2026-06-05
│   │   ├── 2026-06-06
│   │   ├── 2026-06-07
│   │   ├── 2026-06-08
│   │   ├── 2026-06-09
│   │   ├── 2026-06-10
│   │   ├── 2026-06-11
│   │   ├── 2026-06-12
│   │   ├── 2026-06-13
│   │   ├── 2026-06-14
│   │   ├── 2026-06-15
│   │   ├── 2026-06-16
│   │   └── latest -> 2026-06-16
│   └── scheduler
│       ├── 2026-06-08
│       ├── 2026-06-09
│       └── latest -> 2026-06-09
├── plugins
└── README.md
```

## Docker Configuration for Airflow

See: docker-compose.yaml

## Docker Configuration for Airflow's Extended Image

See: Dockerfile

## Docker Configuration for AWS

Create local .env file and add to .gitignore

## Execute Docker Image to Create Containers

```
$ docker-compose up airflow-init
airflow-init-1 exited with code 0
```

Note: This means your Apache Airflow environment initialized successfully.

```
$ docker-compose up --build -d
```

## Airflow DAG and Tasks Creation

See: dags directory

## Execute DAGs from Airflow UI

URL: http://localhost:8080/
Username/Password: airflow

## Access S3 Buckets/Objects using AWS

https://aws.amazon.com/console/

## Debug Hard

1. Airflow Environment Variables
```
/Users/USERNAME/dev/top-gun-airflow/.env: no such file or directory
```
Solution
```
$ touch .env
```
Note: docker-compose.yaml has default values for AIRFLOW environment variables, so this file can be blank in our case.

2. Docker API Connection Error/s
```
unable to get image 'redis:7.2-bookworm': failed to connect to the docker API at unix:///Users/USERNAME/.docker/run/docker.sock; check if the path is correct and if the daemon is running: dial unix /Users/USERNAME/.docker/run/docker.sock: connect: no such file or directory
```
Solution
Mac Top Menu Bar: Check/Ensure “Docker Desktop is running”

3. DAG Import Error: Faker
```
ModuleNotFoundError: No module named 'faker'
```
Solution
Create custom Dockerfile
See: Dockerfile

4. Docker Desktop UI Errors
```
airflow command error: argument GROUP_OR_COMMAND: invalid choice: 'api-server' (choose from 'celery', 'cheat-sheet', 'config', 'connections', 'dag-processor', 'dags', 'db', 'info', 'jobs', 'kerberos', 'plugins', 'pools', 'providers', 'rotate-fernet-key', 'scheduler', 'standalone', 'tasks', 'triggerer', 'variables', 'version', 'webserver'), see help above.
```
Solution
The api-server architecture component was introduced in Airflow 3.0. Because your system is running Airflow 2.x, its command-line interface (CLI) does not recognize this option. Works with Airflow 3.2.2.

5. DAG Import Error: Schedule Interval
```
dag import error TypeError: DAG.__init__() got an unexpected keyword argument 'schedule_interval'
```
Solution
The error TypeError: DAG.__init__() got an unexpected keyword argument 'schedule_interval' occurs because the schedule_interval argument has been completely removed in modern versions of Apache Airflow. Airflow deprecated schedule_interval in version 2.4.0 and formally removed it in later major releases. To fix this import error, replace schedule_interval with the “schedule” argument in your DAG file/s.