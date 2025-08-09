Metrics Server can be installed either directly from YAML manifest or via the official Helm chart. To install the latest Metrics Server release from the components.yaml manifest, run the following command.

```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Disable TLS in metrics. Add these flags under `spec.template.spec.containers.args`

```yml
- --kubelet-insecure-tls
```