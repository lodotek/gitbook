# Pull image from private repository

### Define imagePullSecrets value

`imagePullSecrets` is usually defined if you generate your chart by `helm create`. If no, you will need to define it in values.yaml.

```yaml
# Default values for mychart.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

image:
  repository: mychart
  pullPolicy: IfNotPresent

imagePullSecrets: []
```

### Update deployment template

You need to set imagePullSecrets in deployment.yaml. Again, if you create chart by `helm create`, thing is ready.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  ...
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    ...
  template:
    metadata:
      ...
    spec:
    {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
        {{- toYaml . | nindent 8 }}
    {{- end }}
```

### Set image pull secret value in installation

```bash
helm install --generate-name ./mychart \
  --set imagePullSecrets[0].name=regcred
```
