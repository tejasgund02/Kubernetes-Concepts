# Kubernetes Jobs & CronJobs

## Overview
**Jobs** and **CronJobs** in Kubernetes are used to run tasks that execute to completion. While a **Job** runs a task once or a specified number of times, a **CronJob** schedules tasks to run at specified intervals.

## Jobs
A **Job** ensures that a specified number of Pods successfully complete their execution. It is useful for batch processing and short-lived tasks.

### Use Cases
- Running batch data processing tasks.
- Performing database migrations.
- Executing one-time scripts.

### Creating a Job
Example YAML manifest for a Job:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  completions: 1
  template:
    spec:
      containers:
      - name: example-container
        image: busybox
        command: ["sh", "-c", "echo Hello from Job; sleep 10"]
      restartPolicy: Never
```

### Managing Jobs
#### Creating a Job
```sh
kubectl apply -f job.yaml
```

#### Listing Jobs
```sh
kubectl get jobs
```

#### Viewing Job Logs
```sh
kubectl logs job/example-job
```

#### Deleting a Job
```sh
kubectl delete job example-job
```

## CronJobs
A **CronJob** manages Jobs that run on a scheduled basis, similar to a Unix cron schedule.

### Use Cases
- Scheduling backups.
- Running periodic cleanup tasks.
- Sending scheduled notifications.

### Creating a CronJob
Example YAML manifest for a CronJob:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: example-cronjob
spec:
  schedule: "*/5 * * * *"  # Runs every 5 minutes
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: example-container
            image: busybox
            command: ["sh", "-c", "echo Hello from CronJob"]
          restartPolicy: OnFailure
```

### Managing CronJobs
#### Creating a CronJob
```sh
kubectl apply -f cronjob.yaml
```

#### Listing CronJobs
```sh
kubectl get cronjobs
```

#### Checking CronJob Execution History
```sh
kubectl get jobs --selector=job-name=example-cronjob
```

#### Deleting a CronJob
```sh
kubectl delete cronjob example-cronjob
```

## Conclusion
- **Jobs** are used for one-time or limited-run tasks.
- **CronJobs** schedule Jobs to run at recurring intervals.
- Proper cleanup of completed Jobs is necessary to prevent excessive resource consumption.

## References
- [Kubernetes Documentation - Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [Kubernetes Documentation - CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

