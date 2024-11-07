## Kubernetes JSON Schemas

This repository provides Kubernetes JSON schemas for validating, autocompleting, and enabling IntelliSense for YAML files in editors like Visual Studio Code.

### What are JSON Schemas?

JSON schemas define the expected structure of a document (such as YAML or JSON), ensuring it includes required fields, follows the correct format, and validates data types. With schemas, you can validate configurations as you write them, catching errors before deployment. For example, these schemas can validate Kubernetes CRDs or GitHub Actions workflows to ensure correctness.

### Hosted on GitHub Pages

All schemas in this repository are hosted on GitHub Pages for easy access and integration.

## Prerequisites

To enable schema validation in Visual Studio Code, install the [Red Hat YAML Extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) (`redhat.vscode-yaml`).

## Usage

To use a schema directly in your YAML file, add the `$schema` directive at the top of your document.

### Example

```yaml
# yaml-language-server: $schema=https://flanksio.github.io/kubernetes-json-schemas/dragonflydb.io/dragonfly_v1alpha1.json
---
apiVersion: dragonflydb.io/v1alpha1
kind: Dragonfly
metadata:
  name: dragonfly
spec:
  image: ghcr.io/dragonflydb/dragonfly:v1.24.0
  replicas: 3
  snapshot:
    cron: "*/5 * * * *"
    persistentVolumeClaimSpec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 10Gi
  env:
    - name: MAX_MEMORY
      valueFrom:
        resourceFieldRef:
          resource: limits.memory
          divisor: 1Mi
  args:
    - --maxmemory=$(MAX_MEMORY)Mi
    - --proactor_threads=2
  resources:
    requests:
      cpu: 100m
    limits:
      memory: 2048Mi
```