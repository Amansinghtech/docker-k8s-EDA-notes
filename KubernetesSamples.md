# Kubernetes Commonly Used Workloads, Services, and Configs

## Workloads

### Pods

* **Use:** The most basic unit in Kubernetes. They run one or more containers. Used for running single instances of an application or tightly coupled containers.
* **Note:** Generally, you don't manage pods directly, but through higher-level workloads.
* **Example YAML:**

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: my-pod
    spec:
      containers:
      - name: my-container
        image: nginx:latest
        ports:
        - containerPort: 80
        env:
        - name: MY_ENV_VAR
          value: "my-value"
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "200m"
            memory: "256Mi"
    ```

### Deployments

* **Use:** For managing stateless applications. They ensure a specified number of replica pods are running. They handle rolling updates and rollbacks. Ideal for web servers, APIs, and other applications where multiple identical instances can run.
* **Example YAML:**

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-container
            image: nginx:latest
            ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "200m"
              memory: "256Mi"
      strategy:
        type: RollingUpdate
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 1
    ```

### StatefulSets

* **Use:** For managing stateful applications that require stable network identities, persistent storage, and ordered deployment/scaling. Examples include databases (like MySQL, PostgreSQL), message queues (like Kafka), and distributed data stores.
* **Note:** StatefulSets provide stable pod names and persistent volumes, which are crucial for stateful applications.
* **Example YAML (Simplified with Volume Claim):**

    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: my-statefulset
    spec:
      serviceName: "my-service"
      replicas: 3
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-container
            image: busybox:latest
            command: ["sleep", "3600"]
            volumeMounts:
            - name: data
              mountPath: /data
      volumeClaimTemplates:
      - metadata:
          name: data
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi
    ```

### DaemonSets

* **Use:** For running a pod on every (or some) node in the cluster. Used for cluster-level services like log collectors (Fluentd, Elasticsearch), monitoring agents (Prometheus Node Exporter), and network plugins.
* **Example YAML:**

    ```yaml
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: my-daemonset
    spec:
      selector:
        matchLabels:
          app: my-daemon
      template:
        metadata:
          labels:
            app: my-daemon
        spec:
          containers:
          - name: my-container
            image: fluentd:latest
            volumeMounts:
            - name: varlog
              mountPath: /var/log
          volumes:
          - name: varlog
            hostPath:
              path: /var/log
    ```

### Jobs

* **Use:** For running short-lived, batch-oriented tasks. They ensure a task completes successfully. Ideal for data processing, backups, and one-time scripts.
* **Example YAML:**

    ```yaml
    apiVersion: batch/v1
    kind: Job
    metadata:
      name: my-job
    spec:
      template:
        spec:
          containers:
          - name: my-container
            image: busybox:latest
            command: ["echo", "Hello from my job!"]
          restartPolicy: Never
          volumes:
          - name: my-config-volume
            configMap:
              name: my-config
      volumes:
        - name: my-config-volume
          configMap:
            name: my-config
    ```

### CronJobs

* **Use:** For scheduling Jobs to run at specific intervals, similar to cron in Linux. Used for recurring tasks like scheduled backups, reports, and data synchronization.
* **Example YAML:**

    ```yaml
    apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: my-cronjob
    spec:
      schedule: "*/1 * * * *" # Run every minute
      jobTemplate:
        spec:
          template:
            spec:
              containers:
              - name: my-container
                image: busybox:latest
                command: ["date"]
              restartPolicy: OnFailure
              volumes:
              - name: my-secret-volume
                secret:
                  secretName: my-secret
      volumes:
        - name: my-secret-volume
          secret:
            secretName: my-secret
    ```

## Services

(Same as before, configs are usually within the pods or other related workloads.)

## Configs

### ConfigMaps

* **Use:** To store non-sensitive configuration data as key-value pairs.
* **Example YAML:**

    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-config
    data:
      app.properties: |
        database.url=[mydb.example.com](https://www.google.com/search?q=mydb.example.com)
        app.theme=dark
      ui.properties: |
        color=blue
    ```

### Secrets

* **Use:** To store sensitive information like passwords, API keys, and certificates.
* **Example YAML:**

    ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: my-secret
    type: Opaque
    stringData:
      username: myuser
      password: mypassword
    ```