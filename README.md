# Airflow - Concepts

Note: This code uses Celery Executor running Airflow 1.10.0.

## Dev environment Requirements
Note:IVD (Installed via Docker)

| Requirement | Version
| :---: | :---: | 
| Airflow  | 1.10.12 |
| Docker  | 20.10.7 |
| PostgreSQL (IvD)  | 9.3 |


To run this project on your local environment, it is recommended(was developed with) the hardware specifications.

| Hardware Requirements | Version
| :---: | :---: | 
| Cores  | Minimum 2 |
| RAM  | Minimum 4 GB |


To run the project, run the following command inside the project folder.

```docker-compose -f docker-compose-CeleryExecutor.yml up```

or to run docker-compose in background

```docker-compose -f docker-compose-CeleryExecutor.yml up -d```


__To Do__

Airflow version to be updated to 2.1.3.

This project was developed to understand concepts involved with Airflow DAGs like Branching, Triggering, Templating, XCOMs, External Task Sensors and so on.

1) Airflow Branching

Check_api task checks the APIs ip-api, none, ipinfo, and ipstack, if its available. If it is available, then it will execute a Save task.
If there are more than one API available, the Save task is still executed using the BrachOperator.

Branching ![Alt text](screenshots/branching.png?raw=true "Airflow Branching of Tasks")

2) Triggering branch

Download from Website A and Website B in parallel, process the downloaded data in parallel and merge the data. Depending on some conditions execute either an email notification or Slack notification.

**Cases:**
Case 1:
All the tasks are successful.

All Success ![Alt text](screenshots/screenshot1.png?raw=true "All Success")

Case 2:
When the download fails, but still run both notif_a and notif_b tasks

Download Task Fails ![Alt text](screenshots/screenshot2.png?raw=true "Download task fails")

Case:3
When the initial tasks of downloading fails, the notification task still executes.

Initial Task fails ![Alt text](screenshots/screenshot3.png?raw=true "Initial Task fails")

3) Templating

When using a user stored variable in dags, use {{ var.value.<variable_name_stored_on_ui_Admin->Variables>}}
Create a variable called source_path from Admin->Variables->Create->(Key) source_path,(val)/usr/local/airflow/dags

Templating DAG ![Alt text](screenshots/screenshot4.png?raw=true "Templating")

4) XCOMs

Sharing data between tasks via XCOM for the Dag

Tasks List for XCOMs![Alt text](screenshots/screenshot6.png?raw=true "Tasks List for XCOM")

The XCOM list should look like
``
XCOM List ![Alt text](screenshots/screenshot5.png?raw=true "XCOM List")

5) External Task sensor

A task wait for another task in another dag to be completed.
There are 3 tasks in Dag 1 and 3 tasks in Dag 2.

Task sensor(t3) the first dag in Dag 2, will wait for the dag t3 in first Dag to be completed.

Same schedule interval is maintained between DAGs.
