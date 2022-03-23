
# Building a Continuous Deployment GitOps Pipeline to EKS

This tutorial describes how to set up a basic GitOps deployment pipeline with Flux and GitHub actions. It uses the Kubernetes Guestbook application and also adapts a [GitHub Actions pipeline](https://github.com/github-developer/example-actions-flux-eks) originally developed by Jeremy Adams and John Bohannon of GitHub.


## What do we need for GitOps on AWS?

These are the tools that you’ll be working with in order to create a basic GitOps pipeline for application deployments or even for cluster components. If running this in production, you might also need an additional component to scan your base images for added security.


|         Component         	| Implementation                        	| Notes                                                                                                                               	|
|:-------------------------:	|---------------------------------------	|-------------------------------------------------------------------------------------------------------------------------------------	|
| EKS                       	| [EKSctl](https://eksctl.io/)                                	| Managed Kubernetes service.                                                                                                         	|
| Git repo                  	| https://github.com/weaveworks/guestbook-gitops 	| A Git repository containing your application and cluster manifests files.                                                           	|
| Continuous Integration    	| [GitHub Actions](https://github.com/features/actions)                        	| Test and integrate the code - can be anything from CircleCI to GitHub actions.                                                      	|
| Continuous Delivery       	| [Flux version 2](https://toolkit.fluxcd.io/cmd/flux/)                                  	| Cluster <-> repo synchronization                                                                                                    	|
| Container Registry        	| [AWS ECR Container Registry](https://aws.amazon.com/ecr/)            	| Can be any image registry or even a directory.                                                                                      	|
| GitHub secrets management 	| ECR                                   	| Can use [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets) or [Vault](https://www.vaultproject.io/).In this case we are using Elastic Container Registry (ECR) which provides resource level  security. 	|



## Before you begin:

You will need to sign up for the following services:

* AWS account  with the ability to create EKS clusters
* GitHub account
* ECR Container Registry (we'll step through this below)
* Kubectl version 1.18 or newer
* [Kustomize](https://github.com/kubernetes-sigs/kustomize)

In this example, you will install the standard [Guestbook sample application](https://kubernetes.io/docs/tutorials/stateless-application/guestbook/) to EKS. Then you’ll make a change to the look of the buttons and deploy it to EKS with GitOps.

## Part 1: Fork the Guestbook app repository

You will need a GitHub account for this step.

* Fork and clone the repository: 
* Keep the repo handy as you’ll be adding a deployment key to the repo that Flux requires as well as a few credentials once you have your ECR AWS container registry set up.

## Part 2 : Follow the Medium Blog