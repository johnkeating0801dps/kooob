# Kubernetes Practical – NGINX Deployment

This README covers only the Kubernetes operations required for the practical:

1. Get a container image
2. Create a Deployment
3. Expose the Deployment
4. Show Pods
5. Scale up the Deployment
6. Delete a Pod
7. Show Pods again

The commands below work in Windows Command Prompt / PowerShell and Linux.

## Prerequisites

Make sure Docker Desktop, Minikube and kubectl are installed.

Start Minikube:

```bash
minikube start
```

Check that the cluster is running:

```bash
kubectl get nodes
```

The node should show `Ready`.

## Step 1 – Get an image

Download the NGINX image:

```bash
docker pull nginx
```

Note: Kubernetes can download the image automatically when you create the Deployment. This command is included because the practical may ask you to get an image first.

## Step 2 – Create a Deployment

```bash
kubectl create deployment mynginx --image=nginx
```

This creates a Deployment named `mynginx`. A Deployment manages Pods and maintains the required number of replicas.

Check it:

```bash
kubectl get deployments
```

## Step 3 – Expose the Deployment

```bash
kubectl expose deployment mynginx --type=NodePort --port=80
```

This creates a NodePort Service to make the NGINX Pods accessible.

Check the Service:

```bash
kubectl get services
```

Open the application through Minikube:

```bash
minikube service mynginx
```

Minikube opens the service in a browser. If it displays a URL instead, open that URL manually.

## Step 4 – Show the Pods

```bash
kubectl get pods
```

For additional details:

```bash
kubectl get pods -o wide
```

Wait until the Pod status becomes `Running`.

## Step 5 – Upscale the Deployment

Scale the Deployment to 3 replicas:

```bash
kubectl scale deployment mynginx --replicas=3
```

This tells Kubernetes to maintain three NGINX Pods.

Verify:

```bash
kubectl get deployments
kubectl get pods
```

You should see three Pods belonging to `mynginx`.

## Step 6 – Delete a Pod

List the Pods:

```bash
kubectl get pods
```

Copy the name of one Pod belonging to `mynginx`, then run this command with its actual name:

```bash
kubectl delete pod <pod-name>
```

Example only (the generated name will be different):

```bash
kubectl delete pod mynginx-xxxxxxxxxx-yyyyy
```

Do not type the angle brackets. Replace `<pod-name>` with the real Pod name from `kubectl get pods`.

Because the Pod is managed by a Deployment, Kubernetes automatically creates a replacement Pod to maintain three replicas.

## Step 7 – Show the Pods again

```bash
kubectl get pods
```

After a short wait, you should see three Pods again. The replacement Pod may take a little time to reach `Running`.

## Quick Revision – Commands to Remember

| Task | Command |
|---|---|
| Download image | `docker pull nginx` |
| Create Deployment | `kubectl create deployment mynginx --image=nginx` |
| Expose Deployment | `kubectl expose deployment mynginx --type=NodePort --port=80` |
| Show Deployments | `kubectl get deployments` |
| Show Services | `kubectl get services` |
| Show Pods | `kubectl get pods` |
| Scale to 3 Pods | `kubectl scale deployment mynginx --replicas=3` |
| Delete a Pod | `kubectl delete pod <pod-name>` |
| Show Pods again | `kubectl get pods` |

## Optional – Clean up after the practical

```bash
kubectl delete service mynginx
kubectl delete deployment mynginx
minikube stop
```

Remember: delete the Pod using its actual name. Since it belongs to a Deployment, Kubernetes replaces it automatically.
