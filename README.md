# Kubernetes Pod Specifications Manual

This repository provides detailed examples and documentation on Kubernetes Pod specifications, including essential fields, configurations, and advanced options.

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Pod Specification](#basic-pod-specification)
3. [Pod Spec Fields](#pod-spec-fields)
    - [Metadata](#metadata)
    - [Containers](#containers)
    - [Volumes](#volumes)
    - [Restart Policy](#restart-policy)
    - [Affinity and Node Selector](#affinity-and-node-selector)
    - [Tolerations](#tolerations)
    - [Lifecycle Hooks](#lifecycle-hooks)
    - [Probes](#probes)
4. [Examples](#examples)
5. [Useful Commands](#useful-commands)
6. [Contributing](#contributing)
7. [License](#license)

---

## Introduction

In Kubernetes, a **Pod** is the smallest deployable unit, representing a group of one or more containers sharing networking and storage. This manual provides a comprehensive overview of Kubernetes Pod specifications, covering basic configurations as well as more advanced fields for customizing Pod behavior and deployment.

---

## Basic Pod Specification

A basic Kubernetes Pod YAML file (`pod.yaml`) contains the following structure:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx:latest
Explanation
apiVersion: Defines the API version (v1 for Pods).
kind: Specifies the resource type, Pod.
metadata: Holds metadata such as the Pod's name, labels, and annotations.
spec: The main section where the Pod's desired state is defined, including container images, ports, and other settings.

Pod Spec Fields
Metadata
The metadata field defines basic information about the Pod, including its name, labels, and annotations.

name: Unique name for the Pod.
namespace: (Optional) Namespace in which the Pod will be created.
labels: Key-value pairs to categorize and organize Pods.
annotations: Non-identifying metadata often used for auxiliary information.

Example

metadata:
  name: example-pod
  labels:
    app: web
  annotations:
    owner: team-a

---

## Containers
The containers field is a list of container specifications for the Pod. Each container should have a unique name, an image, and any necessary configurations such as ports, resources, and environment variables.

name: Name of the container.
image: Docker image to run.
ports: Ports to expose.
env: Environment variables to pass to the container.
resources: CPU and memory limits and requests.
volumeMounts: Mount paths for volumes.

containers:
  - name: nginx-container
    image: nginx:latest
    ports:
      - containerPort: 80
    env:
      - name: APP_ENV
        value: production
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"

## Volumes
Volumes allow data to persist across container restarts and can be shared among containers in a Pod.

Example

volumes:
  - name: storage-volume
    emptyDir: {}

## Restart Policy
Defines when Kubernetes should restart a container:

Always: Always restart (default).
OnFailure: Restart only if the container exits with an error.
Never: Do not restart the container.
Example
restartPolicy: OnFailure

## Affinity and Node Selector
Controls where Pods are scheduled within the cluster.

nodeSelector: Assigns Pods to nodes with matching labels.
affinity: Provides more advanced scheduling options, such as preferred or required affinity.

Example
nodeSelector:
  disktype: ssd

## Tolerations
Tolerations allow Pods to be scheduled on nodes with matching taints, defining rules for which nodes the Pod can tolerate.

Example
tolerations:
  - key: "example-key"
    operator: "Equal"
    value: "example-value"
    effect: "NoSchedule"

## Lifecycle Hooks
Lifecycle hooks allow you to trigger actions at different stages of the container lifecycle, such as postStart or preStop.

Example

lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "echo Goodbye"]

## Probes
Liveness and Readiness probes are used to check the health and readiness of containers:

livenessProbe: Restarts containers that become unresponsive.
readinessProbe: Ensures a container is ready to receive traffic.

Example
livenessProbe:
  httpGet:
    path: /health
    port: 80
  initialDelaySeconds: 3
  periodSeconds: 5

## Examples
Example 1: Basic Pod

apiVersion: v1
kind: Pod
metadata:
  name: basic-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80

## Example 2: Pod with Multi-Container Setup
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
    - name: app-container
      image: my-app:1.0
    - name: logging-container
      image: busybox
      args: ["sh", "-c", "tail -f /var/log/app.log"]

## Example 3: Pod with Resource Limits and Environment Variables

apiVersion: v1
kind: Pod
metadata:
  name: resource-limits-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
      env:
        - name: ENVIRONMENT
          value: production

## Useful Commands
Here are some helpful kubectl commands to manage Pods:

Create a Pod: kubectl apply -f pod.yaml
Delete a Pod: kubectl delete pod <pod-name>
Get Pod details: kubectl describe pod <pod-name>
List all Pods: kubectl get pods
Get container logs: kubectl logs <pod-name> -c <container-name>
Execute a command in a container: kubectl exec -it <pod-name> -- <command>

##Contributing
Feel free to submit issues or pull requests to enhance the examples or add new documentation. Contributions are welcome!

##License
This project is licensed under the MIT License.




