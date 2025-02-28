# Kubernetes Cheat Sheet: Commonly Used Commands

## Pods

* **List pods:**
    ```bash
    kubectl get pods
    kubectl get po -A # All namespaces
    ```
* **Describe a pod:**
    ```bash
    kubectl describe pod <pod-name>
    ```
* **Get pod logs:**
    ```bash
    kubectl logs <pod-name>
    kubectl logs -f <pod-name> # Follow logs
    kubectl logs <pod-name> -c <container-name> # Specific container
    ```
* **Execute a command in a pod:**
    ```bash
    kubectl exec -it <pod-name> -- /bin/bash
    kubectl exec -it <pod-name> -c <container-name> -- /bin/sh
    ```
* **Delete a pod:**
    ```bash
    kubectl delete pod <pod-name>
    ```

## Deployments

* **List deployments:**
    ```bash
    kubectl get deployments
    kubectl get deploy -A
    ```
* **Describe a deployment:**
    ```bash
    kubectl describe deployment <deployment-name>
    ```
* **Scale a deployment:**
    ```bash
    kubectl scale deployment <deployment-name> --replicas=<number>
    ```
* **Update a deployment (rolling update):**
    ```bash
    kubectl apply -f <deployment.yaml>
    ```
* **Delete a deployment:**
    ```bash
    kubectl delete deployment <deployment-name>
    ```
* **View rollout status:**
    ```bash
    kubectl rollout status deployment/<deployment-name>
    ```
* **View rollout history:**
    ```bash
    kubectl rollout history deployment/<deployment-name>
    ```
* **Undo a rollout:**
    ```bash
    kubectl rollout undo deployment/<deployment-name>
    ```

## Services

* **List services:**
    ```bash
    kubectl get services
    kubectl get svc -A
    ```
* **Describe a service:**
    ```bash
    kubectl describe service <service-name>
    ```
* **Delete a service:**
    ```bash
    kubectl delete service <service-name>
    ```

## Nodes

* **List nodes:**
    ```bash
    kubectl get nodes
    ```
* **Describe a node:**
    ```bash
    kubectl describe node <node-name>
    ```
* **Cordon a node (mark as unschedulable):**
    ```bash
    kubectl cordon <node-name>
    ```
* **Uncordon a node (mark as schedulable):**
    ```bash
    kubectl uncordon <node-name>
    ```
* **Drain a node (evict pods):**
    ```bash
    kubectl drain <node-name> --ignore-daemonsets
    ```

## Namespaces

* **List namespaces:**
    ```bash
    kubectl get namespaces
    kubectl get ns
    ```
* **Create a namespace:**
    ```bash
    kubectl create namespace <namespace-name>
    ```
* **Delete a namespace:**
    ```bash
    kubectl delete namespace <namespace-name>
    ```
* **Run command in a specific namespace:**
    ```bash
    kubectl get pods -n <namespace-name>
    ```

## General

* **Apply a configuration file:**
    ```bash
    kubectl apply -f <filename.yaml>
    kubectl apply -f <directory> # apply all yaml files in a directory
    ```
* **Create from a configuration file:**
    ```bash
    kubectl create -f <filename.yaml>
    ```
* **Delete from a configuration file:**
    ```bash
    kubectl delete -f <filename.yaml>
    ```
* **Get resource in YAML format:**
    ```bash
    kubectl get pod <pod-name> -o yaml
    ```
* **Get resource in JSON format:**
    ```bash
    kubectl get pod <pod-name> -o json
    ```
* **Get current context:**
    ```bash
    kubectl config current-context
    ```
* **Switch context:**
    ```bash
    kubectl config use-context <context-name>
    ```
* **Get events:**
    ```bash
    kubectl get events
    ```
* **Explain a resource:**
    ```bash
    kubectl explain pod
    kubectl explain deployment.spec.replicas
    ```