# Kubectl commands
- [Introduction](#introduction)
- [Kubectl Cheat Sheets](#kubectl-cheat-sheets)
- [Kubectl explain](#kubectl-explain)
- [Kubectl Autocomplete](#kubectl-autocomplete)
- [List all resources and sub resources that you can constrain with RBAC](#list-all-resources-and-sub-resources-that-you-can-constrain-with-rbac)
- [Copy a configMap in kubernetes between namespaces](#copy-a-configmap-in-kubernetes-between-namespaces)
- [Copy secrets in kubernetes between namespaces](#copy-secrets-in-kubernetes-between-namespaces)
- [Export resources with kubectl and python](#export-resources-with-kubectl-and-python)
- [Buildkit CLI for kubectl a drop in replacement for docker build](#buildkit-cli-for-kubectl-a-drop-in-replacement-for-docker-build)
- [Kubectl Alternatives](#kubectl-alternatives)
    - [Manage Kubernetes (K8s) objects with Ansible Kubernetes Module](#manage-kubernetes-k8s-objects-with-ansible-kubernetes-module)
    - [Jenkins Kubernetes Plugins](#jenkins-kubernetes-plugins)

## Introduction
* [itnext.io: Boosting your kubectl productivity](https://itnext.io/boosting-your-kubectl-productivity-b348f7c25712)
* [medium: 4 Simple Kubernetes Terminal Customizations to Boost Your Productivity](https://medium.com/better-programming/4-simple-kubernetes-terminal-customizations-to-boost-your-productivity-deda60a19924)
* [medium: Ready-to-use commands and tips for kubectl](https://medium.com/flant-com/kubectl-commands-and-tips-7b33de0c5476)
* [medium: Be fast with Kubectl 1.19 CKAD/CKA 🌟](https://medium.com/faun/be-fast-with-kubectl-1-18-ckad-cka-31be00acc443) Collection of the fastest ways to create k8s resources using kubectl ≥ 1.18
* [developers.redhat.com: Kubectl: Developer tips for the Kubernetes command line 🌟](https://developers.redhat.com/blog/2020/11/20/kubectl-developer-tips-for-the-kubernetes-command-line/)
* [ibm.com: 8 Kubernetes Tips and Tricks 🌟](https://www.ibm.com/cloud/blog/8-kubernetes-tips-and-tricks) Most of the tips given below are using kubectl, a powerful command-line tool that allows you to execute commands against Kubernetes clusters.
    * Set default namespaces
    * Helpful aliases to save time
    * YAML editing with vi
    * Create YAML from kubectl commands 
    * Switching between Kubernetes namespaces
    * Shell auto-completion
    * Viewing resource utilization
    * Extend kubectl and create your own commands using raw outputs
* [pixelstech.net: Update & Delete Kubernetes resources in one-line command](https://www.pixelstech.net/article/1604225312-Update-&amp-Delete-Kubernetes-resources-in-one-line-command)
* [opensource.com: 5 useful ways to manage Kubernetes with kubectl](https://opensource.com/article/21/7/kubectl) Learn kubectl to enhance how you interact with Kubernetes.
* [hackerxone.com: How to Manage Single & Multiple Kubernetes Clusters using kubectl & kubectx in Linux](https://www.hackerxone.com/2021/07/10/how-manage-single-multiple-kubernetes-clusters-using-kubectl-kubectx-linux/)

## Kubectl Cheat Sheets
* [Kubectl Cheat Sheets](cheatsheets.md)

## Kubectl explain
- [kubectl explain](https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_explain/)
- [itnext.io: Using ‘kubectl explain’ for Custom Resources](https://itnext.io/understanding-kubectl-explain-9d703396cc8) Goal: Explore if ‘kubectl explain’ can be used to discover static information about Custom Resources

```for r in $(kubectl api-resources|grep -v ^N|awk '{print $1}');do kubectl explain $r --recursive;done```

## Kubectl Autocomplete
* [Kubectl Autocomplete](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [kubectl Shell Autocomplete](https://blog.heptio.com/kubectl-shell-autocomplete-heptioprotip-48dd023e0bf3)
* [Kubernetes productivity tips and tricks 🌟](https://www.padok.fr/en/blog/kubernetes-productivity-tips)
* [complete-alias](https://github.com/cykerway/complete-alias) Automagical shell alias completion.

```bash
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```

You can also use a shorthand alias for kubectl that also works with completion:

```bash
alias k=kubectl
complete -F __start_kubectl k
```

## List all resources and sub resources that you can constrain with RBAC
* kind of a handy way to see all thing things you can affect with Kubernetes RBAC. This will list all resources and sub resources that you can constrain with RBAC. If you want to see just subresources append "| grep {name}/":

```bash
kubectl get --raw /openapi/v2  | jq '.paths | keys[]'
```

## Copy a configMap in kubernetes between namespaces
* Copy a configMap in kubernetes between namespaces with deprecated "--export" flag:

```bash
kubectl get configmap --namespace=<source> <configmap> --export -o yaml | sed "s/<source>/<dest>/" | kubectl apply --namespace=<dest> -f -
```

* [Flag export deprecated in kubernetes 1.14](https://github.com/kubernetes/kubernetes/pull/73787). Instead following command can be used:

```bash
kubectl get configmap <configmap-name> --namespace=<source-namespace> -o yaml | sed ‘s/namespace: <from-namespace>/namespace: <to-namespace>/’ | kubectl create -f
```

* Reference: [Copy a configMap in kubernetes between namespaces](https://stackoverflow.com/questions/55515594/is-there-a-way-to-share-a-configmap-in-kubernetes-between-namespaces)
  
## Copy secrets in kubernetes between namespaces
* [Copy secrets between namespaces](https://stackoverflow.com/questions/55515594/is-there-a-way-to-share-a-configmap-in-kubernetes-between-namespaces):

```bash
kubectl get secret <secret-name> --namespace=<source> -o yaml | sed ‘s/namespace: <from-namespace>/namespace: <to-namespace>/’ | kubectl create -f
```

## Export resources with kubectl and python
* Export resources with [zoidbergwill/export.sh](https://gist.github.com/zoidbergwill/6af8c80cc5b706e2adcf25df3dc2f7e1#file-export_resources-py), by [zoidbergwill](https://gist.github.com/zoidbergwill)

## Buildkit CLI for kubectl a drop in replacement for docker build
- [container-registry.com: Lifting Developers’ Productivity 🌟](https://container-registry.com/posts/productivity-lift-buildkit-cli-for-kubectl/) With BuildKit CLI for kubectl a drop in replacement for docker build. In this post, you will learn how to build container images with BuildKit CLI for kubectl (a replacement for the `docker build` command)
- [vmware-tanzu/buildkit-cli-for-kubectl (kubectl plugin)](https://github.com/vmware-tanzu/buildkit-cli-for-kubectl) BuildKit CLI for kubectl is a tool for building container images with your Kubernetes cluster.

## Kubectl Alternatives
* [Helm and Kubernetes](#helm-kubernetes-tool)
* [Kubectl plugins and tools](#kubectl-plugins)

### Manage Kubernetes (K8s) objects with Ansible Kubernetes Module
* [Manage Kubernetes (K8s) objects](https://docs.ansible.com/ansible/latest/modules/k8s_module.html)
* [ansibleforkubernetes.com 🌟](https://www.ansibleforkubernetes.com/)

### Jenkins Kubernetes Plugins
* [Jenkins Kubernetes Plugin](https://plugins.jenkins.io/kubernetes/)
* [Kubernetes Continuous Deploy](https://plugins.jenkins.io/kubernetes-cd/)