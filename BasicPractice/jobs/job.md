# Kubernetes Job

A **Job** in Kubernetes is used to run **batch processes** that execute to completion. Unlike Deployments or StatefulSets (which manage long-running applications), a Job ensures that a **specified number of pods successfully complete their tasks**.

## 🔹 Key Features of a Kubernetes Job
1. **Ensures Completion** – Runs one or more pods until a specified number of successful completions.
2. **Pod Restart on Failure** – If a pod fails, Kubernetes restarts it (based on backoff policies).
3. **Parallelism Control** – Can run multiple pods in parallel for faster processing.
4. **Automatic Cleanup** – Can delete completed jobs (if configured) to free resources.

## 🔹 Types of Jobs
1. **Single Execution Job**  
   - Runs a pod once and completes.  
   - Example: Data processing task.  

2. **Parallel Job**  
   - Runs multiple pods simultaneously (useful for tasks like data crunching).  

3. **Completion-Based Parallel Job**  
   - Runs N pods, ensuring a set number successfully complete.  

4. **Indexed Job (New in Kubernetes 1.21+)**  
   - Assigns unique indexes to each pod (useful for parallel computations).  

## 🔹 Example: Simple Kubernetes Job
Here’s a basic Job that runs a pod to print "Hello, Kubernetes!" and exits:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: hello-job
spec:
  template:
    spec:
      containers:
      - name: hello-container
        image: busybox
        command: ["echo", "Hello, Kubernetes!"]
      restartPolicy: Never
```

### Key Parts:
- The `command: ["echo", "Hello, Kubernetes!"]` runs once and exits.
- `restartPolicy: Never` prevents the pod from restarting after completion.

## 🔹 Managing Jobs
### Create the Job:
```sh
kubectl apply -f job.yaml
```

### Check Job Status:
```sh
kubectl get jobs
```

### Check Logs:
```sh
kubectl logs <pod-name>
```

### Delete Job:
```sh
kubectl delete job hello-job
```

<!-- Would you like an example of a **CronJob**, which is a Job that runs on a schedule? 🚀 -->

