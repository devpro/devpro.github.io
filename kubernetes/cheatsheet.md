# Kubernetes cheat sheet

[Home](../readme.md) > [Cheatsheet](./cheatsheet.md)

## Kubectl - The Kubernetes CLI

[Overview](https://kubernetes.io/docs/reference/kubectl/overview/), [Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/), [Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

> The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

`kubectl version`

`kubectl cluster-info`

`kubectl config get-clusters`

`kubectl config get-contexts`

`kubectl config use-context <cluster-name>`

`kubectl get services`

`kubectl get nodes`

`watch kubectl get pod --all-namespaces`

`kubectl describe pod <pod-name> --namespace <namespace>`

`kubectl proxy`
