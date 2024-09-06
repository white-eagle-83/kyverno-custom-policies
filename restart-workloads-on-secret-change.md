How this policy works:

This policy ensures that each specified pod is restarted when a secret is updated within the same namespace. Kyverno monitors each secret in every namespace for the annotation restart-workloads: "true". If this annotation is not present in the secret's definition, the secret update will not impact any related resources in any namespace. To activate this policy, create the secret in the Kubernetes cluster as illustrated in the example below.

---
apiVersion: v1
kind: Secret
metadata:
  name: annotated-secret-example
  namespace: demo
  annotations:
    retrigger: "true"
    restart-deployments: "deployment-0,deployment-1"
    restart-statefulsets: "statefulset-2,statefulset-0" 
    restart-daemonsets: "daemonset-2,daemonset-1"
data:
  # your encrypted data here
  username: YWRtaW4=
  password: cmFqLXJhamVzaHZhcmkK

This policy does not fail if the specified annotations of the given secret do not match any deployment, statefulset, or daemonset. It is the responsibility of the resource maintainers to ensure that the targeted resource exists within the cluster.
