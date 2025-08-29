kustomize-k8s-demo
This repository demonstrates how to use Kustomize to manage Kubernetes configurations for different environments (Development and Production) and showcases various patching strategies.

Table of Contents
Introduction to Kustomize

Repository Structure

Base Configuration

Development Environment (overlays/dev)

Production Environment (overlays/prod)

Applying Kustomizations

Patching Strategies Demonstrated

Strategic Merge Patch

JSON Patch

Prerequisites

1. Introduction to Kustomize
Kustomize is a standalone tool to customize Kubernetes objects through a kustomization file. It lets you write and reuse base configurations and then apply environment-specific overlays without directly modifying the original YAMLs. This approach promotes a "write once, use many" philosophy and keeps your configurations clean and manageable.

2. Repository Structure
The repository is organized as follows:

kustomize-k8s-demo/
├── base/                   # Contains the common, uncustomized Kubernetes resources.
├── overlays/               # Contains environment-specific customizations.
│   ├── dev/                # Overlays for the development environment.
│   └── prod/               # Overlays for the production environment.
├── README.md               # This file.
└── .gitignore

3. Base Configuration
The base/ directory contains the fundamental Kubernetes YAMLs for a simple Nginx application:

deployment.yaml: Defines an Nginx Deployment with basic settings.

service.yaml: Defines a ClusterIP Service to expose the Nginx Deployment.

kustomization.yaml: Simply lists the deployment.yaml and service.yaml as resources.

4. Development Environment (overlays/dev)
The overlays/dev/ directory customizes the base for development:

kustomization.yaml:

References the ../../base directory.

Sets a namespace of dev-env for all resources.

Adds a namePrefix of dev- to all resource names.

Includes dev-ingress.yaml as a resource.

Applies a dev-replicas-patch.yaml to the nginx-app Deployment.

dev-replicas-patch.yaml: (Strategic Merge Patch)

Sets the replicas to 1.

Adds NODE_ENV=development and DEBUG_MODE=true environment variables to the Nginx container.

dev-ingress.yaml: An Ingress resource configured for dev.example.com.

To build the Dev environment:

kustomize build overlays/dev

5. Production Environment (overlays/prod)
The overlays/prod/ directory provides production-specific customizations:

kustomization.yaml:

References the ../../base directory.

Sets a namespace of prod-env for all resources.

Adds a namePrefix of prod- to all resource names.

Includes prod-ingress.yaml and prod-configmap.yaml as resources.

Applies three patches to the nginx-app Deployment:

prod-replicas-image-patch.yaml (Strategic Merge Patch)

prod-resources-json-patch.yaml (JSON Patch)

prod-configmap-patch.yaml (Strategic Merge Patch)

prod-replicas-image-patch.yaml: (Strategic Merge Patch)

Increases replicas to 3.

Sets a specific, stable Nginx image version (nginx:1.23.4).

Sets NODE_ENV=production environment variable.

prod-resources-json-patch.yaml: (JSON Patch)

This demonstrates a JSON patch to precisely replace resource limits for CPU and memory. JSON patches are useful for granular modifications, including adding, replacing, or removing specific fields or array elements.

prod-configmap.yaml: A ConfigMap resource containing production-specific Nginx configuration.

prod-configmap-patch.yaml: (Strategic Merge Patch)

Mounts the prod-configmap.yaml as a volume into the Nginx container.

prod-ingress.yaml: An Ingress resource configured for app.example.com with TLS enabled.

To build the Prod environment:

kustomize build overlays/prod

6. Applying Kustomizations
You can apply the Kustomize output directly to your Kubernetes cluster:

For Development:

kustomize build overlays/dev | kubectl apply -f -

For Production:

kustomize build overlays/prod | kubectl apply -f -

7. Patching Strategies Demonstrated
Kustomize supports different ways to patch your base resources:

Strategic Merge Patch
Used in: dev-replicas-patch.yaml, prod-replicas-image-patch.yaml, prod-configmap-patch.yaml

How it works: This is the most common and generally recommended patching strategy. It merges fields based on their type. For scalar fields, the patch overwrites the base. For maps, fields are merged. For lists of primitives, the list is replaced. For lists of objects (like containers or volumes), Kustomize often tries to "merge by key" if the objects have a common identifying field (e.g., name for containers). If no key is defined, the list is replaced.

Example: We use it to modify replicas, image, and add environment variables and volume mounts.

JSON Patch
Used in: prod-resources-json-patch.yaml

How it works: JSON Patch (RFC 6902) provides granular control over modifications. It uses a sequence of operations like add, remove, replace, copy, move, and test. It's particularly useful for:

Modifying specific elements within a list that doesn't have a natural merge key.

Precisely adding or removing fields.

Performing conditional modifications.

Example: We use a replace operation to precisely set the CPU and memory limits for the Nginx container, demonstrating fine-grained control that might be harder to achieve with strategic merge if the structure is complex or arrays need specific element manipulation.

8. Prerequisites
Kubectl: The Kubernetes command-line tool.

Kustomize: You can install Kustomize as a standalone binary (recommended) or use the version built into kubectl (versions 1.14+).

To install standalone Kustomize: go install sigs.k8s.io/kustomize/kustomize/v5@latest (or check the official Kustomize documentation).

kubectl kustomize is also available for newer kubectl versions.