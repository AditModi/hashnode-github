---
title: "Orchestrating Amazon EMR Jobs with AWS Step Functions and Apache Airflow/MWAA"
datePublished: Sat Feb 25 2023 08:49:52 GMT+0000 (Coordinated Universal Time)
cuid: clkcccafi001c09kz6cus7y7r
slug: orchestrating-amazon-emr-jobs-with-aws-step-functions-and-apache-airflowmwaa
tags: aws, big-data, airflow, emr

---

## Introduction

In the world of big data and data processing, Amazon EMR (Elastic MapReduce) has emerged as a powerful service for running distributed data processing jobs at scale. EMR simplifies the process of setting up, managing, and scaling clusters for processing vast amounts of data using popular frameworks like Apache Spark and Hadoop.

However, orchestrating and coordinating complex data workflows can be a challenge. That's where AWS Step Functions and Apache Airflow (or Managed Workflows for Apache Airflow - MWAA) come into play. These two powerful tools provide robust capabilities for managing and automating EMR jobs and other data processing tasks, ensuring seamless execution and better control over data workflows.

## Overview of AWS Step Functions

AWS Step Functions is a serverless workflow service that allows you to build and coordinate applications and microservices using visual workflows. It enables you to design, run, and visualize workflows by stitching together AWS services and custom code.

Step Functions allows you to define state machines that describe the steps and sequence of your workflow. Each step in the workflow represents a state, and transitions between states are controlled by conditions and results. Step Functions ensures that your workflow maintains a consistent and reliable state as it progresses, even if individual steps fail or encounter errors.

AWS Step Functions offers several advantages for orchestrating EMR jobs and other data processing tasks:

1. **Serverless and Scalable:** Step Functions is a fully managed service, meaning you don't need to worry about provisioning or managing any servers. It automatically scales to handle any number of workflows and supports high throughput, making it suitable for processing large volumes of data.
    
2. **Visual Workflow Designer:** Step Functions provides a visual interface to design and visualize your workflows. This visual representation makes it easier to understand the flow of your data processing tasks and enables collaboration among teams.
    
3. **Fault Tolerance and Error Handling:** Step Functions automatically handles errors and retries failed steps. If a particular step encounters an error, Step Functions can automatically retry the step or trigger error handling mechanisms to gracefully recover from failures.
    
4. **Workflow Monitoring and Logging:** Step Functions provides built-in monitoring and logging capabilities, allowing you to track the progress of your workflows and troubleshoot any issues that may arise.
    

## Introduction to Apache Airflow/MWAA

Apache Airflow is an open-source platform to programmatically author, schedule, and monitor workflows. It allows you to define complex workflows as Directed Acyclic Graphs (DAGs) and manage the dependencies between tasks. Airflow provides a rich set of operators for various AWS services, including EMR, making it a popular choice for orchestrating data workflows in the cloud.

Managed Workflows for Apache Airflow (MWAA) is a fully managed service by AWS that runs Apache Airflow in a serverless manner. With MWAA, you no longer need to worry about managing Airflow infrastructure; AWS takes care of it, allowing you to focus on building and running your workflows.

Apache Airflow and MWAA offer several advantages for orchestrating EMR jobs and other data processing tasks:

1. **Flexible and Extensible:** Apache Airflow allows you to define your data workflows as Python code, giving you the flexibility to create complex data pipelines with custom logic and dependencies. The large community behind Airflow also means that you can find a wide range of operators and plugins to extend its functionality.
    
2. **Dependency Management:** Airflow's DAGs allow you to define the dependencies between tasks, ensuring that data processing tasks are executed in the correct order. This dependency management ensures that tasks are executed only when their dependencies are met, minimizing delays and improving overall efficiency.
    
3. **Built-in Scheduling:** Airflow comes with a built-in scheduler that allows you to schedule your data processing tasks at specified intervals or based on specific triggers. This feature ensures that your data workflows run at the desired frequency without manual intervention.
    
4. **Monitoring and Alerting:** Airflow provides a web-based user interface for monitoring the progress of your workflows, allowing you to view task statuses and logs in real-time. Additionally, you can set up alerts and notifications to be informed of any issues or failures in your data pipelines.
    

## Orchestrating EMR Jobs with AWS Step Functions

Let's take a look at how we can use AWS Step Functions to orchestrate Amazon EMR jobs. In this example, we will create a simple workflow that triggers an EMR job and waits for it to complete before proceeding to the next step.

```json
{
  "Comment": "An example workflow to run an EMR job",
  "StartAt": "StartEMRJob",
  "States": {
    "StartEMRJob": {
      "Type": "Task",
      "Resource": "arn:aws:states:::elasticmapreduce:createCluster.sync",
      "Parameters": {
        "Name": "MyEMRJob",
        "ReleaseLabel": "emr-6.5.0",
        "Instances": {
          "InstanceGroups": [
            {
              "InstanceRole": "MASTER",
              "InstanceCount": 1,
              "InstanceType": "m5.xlarge",
              "Market": "ON_DEMAND"
            }
          ],
          "Ec2KeyName": "my-key-pair",
          "KeepJobFlowAliveWhenNoSteps": true},
        "Applications": [
          {
            "Name": "Spark"
          }
        ],
        "VisibleToAllUsers": true},
      "Next": "WaitForEMRJobCompletion"
    },
    "WaitForEMRJobCompletion": {
      "Type": "Task",
      "Resource": "arn:aws:states:::elasticmapreduce:describeCluster.sync",
      "Parameters": {
        "ClusterId.$": "$.CreateClusterClusterId"
      },
      "ResultPath": "$.ClusterStatus",
      "TimeoutSeconds": 3600,
      "Next": "CheckEMRJobStatus"
    },
    "CheckEMRJobStatus": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.ClusterStatus.Cluster.Status.State",
          "StringEquals": "WAITING",
          "Next": "WaitForEMRJobCompletion"
        },
        {
          "Variable": "$.ClusterStatus.Cluster.Status.State",
          "StringEquals": "RUNNING",
          "Next": "WaitForEMRJobCompletion"
        }
      ],
      "Default": "EMRJobComplete"
    },
    "EMRJobComplete": {
      "Type": "Succeed"
    }
  }
}
```

In this Step Functions state machine, we use the `elasticmapreduce:createCluster.sync` task to create an EMR cluster with a single master node of type `m5.xlarge`. We specify the `Spark` application to be installed on the cluster to enable Spark jobs. Once the EMR cluster is created, we use the `elasticmapreduce:describeCluster.sync` task to wait for the EMR job to complete. The state machine checks the status of the EMR cluster and waits until the cluster status is either `WAITING` or `RUNNING`. Once the job is complete, the state machine reaches the `EMRJobComplete` state and succeeds.

This simple Step Functions state machine triggers an EMR job and waits for its completion, providing a basic example of orchestrating EMR jobs using Step Functions.

## Orchestrating EMR Jobs with Apache Airflow/MWAA

Now, let's explore how we can orchestrate EMR jobs using Apache Airflow or Managed Workflows for Apache Airflow (MWAA). In this example, we will create a DAG in Apache Airflow that triggers an EMR job and waits for its completion.

```python
from datetime import datetime, timedelta
from airflow import DAG
from airflow.providers.amazon.aws.operators.emr_create_job_flow import EmrCreateJobFlowOperator
from airflow.providers.amazon.aws.sensors.emr_job_flow import EmrJobFlowSensor
from airflow.operators.dummy_operator import DummyOperator

# Default arguments for the DAG
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

# DAG definition
dag = DAG(
    'orchestrate_emr_jobs',
    default_args=default_args,
    description='Orchestrate EMR jobs with Apache Airflow',
    schedule_interval=timedelta(days=1),
    start_date=datetime(2023, 7, 1),
    catchup=False,
)

# Create EMR cluster
create_emr_cluster = EmrCreateJobFlowOperator(
    task_id='create_emr_cluster',
    job_flow_overrides={
        'Name': 'MyEMRJob',
        'ReleaseLabel': 'emr-6.5.0',
        'Instances': {
            'InstanceGroups': [
                {
                    'InstanceRole': 'MASTER',
                    'InstanceCount': 1,
                    'InstanceType': 'm5.xlarge',
                    'Market': 'ON_DEMAND'
                }
            ],
            'Ec2KeyName': 'my-key-pair',
            'KeepJobFlowAliveWhenNoSteps': True
        },
        'Applications': [
            {
                'Name': 'Spark'
            }
        ],
        'VisibleToAllUsers': True
    },
    dag=dag,
)

# Wait for EMR job completion
wait_for_emr_job_completion = EmrJobFlowSensor(
    task_id='wait_for_emr_job_completion',
    job_flow_id="{{ task_instance.xcom_pull(task_ids='create_emr_cluster')['JobFlowId'] }}",
    dag=dag,
)

# Define task dependencies
create_emr_cluster >> wait_for_emr_job_completion
```

In this Apache Airflow DAG, we use the `EmrCreateJobFlowOperator` to create the EMR cluster, passing the necessary configuration parameters. We then use the `EmrJobFlowSensor` to wait for the EMR job to complete. The `EmrJobFlowSensor` continuously polls the status of the EMR job until it reaches a terminal state, indicating that the job is complete.

By using Apache Airflow or MWAA to orchestrate EMR jobs, you gain more control over your data workflows, enabling you to define complex dependencies, schedule tasks, and monitor their progress in real-time.

## Conclusion

Orchestrating Amazon EMR jobs with AWS Step Functions and Apache Airflow/MWAA provides powerful automation and control over your data processing workflows. AWS Step Functions allows you to design and visualize workflows using state machines, while Apache Airflow/MWAA enables a more programmatic approach to define complex data pipelines. By leveraging these tools, you can automate, schedule, and monitor your EMR jobs effectively, ensuring seamless execution and efficient data processing. With AWS Step Functions and Apache Airflow/MWAA, you can unleash the full potential of Amazon EMR for your big data processing needs.

And if you haven't yet, make sure to follow me on below handles:

👋 connect with me on [LinkedIn](https://www.linkedin.com/in/adit-n-modi-356606261/) 🤓 connect with me on [Twitter](https://twitter.com/adi_12_modi)🐱‍💻 follow me on [github](https://github.com/AditModi) ✍️ Do Checkout [my blogs](https://aditmodi.com)

Like, share, and follow me 🚀 to stay updated with the latest content and to join a vibrant community of tech enthusiasts. Your support is greatly appreciated!