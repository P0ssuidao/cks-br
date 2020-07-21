# 10% - Cluster Setup

## **Use Network security policies to restrict cluster level access**

Network policy to deny all:
```
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress

```

- https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/

- https://kubernetes.io/docs/concepts/services-networking/network-policies/


## **Use CIS benchmark to review the security configuration of Kubernetes components (etcd, kubelet, kubedns, kubeapi)**

The Center for Internet Security (CIS) releases benchmarks for best practice security recommendations. The CIS Kubernetes Benchmark is a set of recommendations for configuring Kubernetes to support a strong security posture. The Benchmark is tied to a specific Kubernetes release.

- https://cloud.google.com/kubernetes-engine/docs/concepts/cis-benchmarks?hl=it
- https://www.cisecurity.org/benchmark/kubernetes/
- https://github.com/aquasecurity/kube-bench

## **Properly set up Ingress objects with security control**

Secret:
```
kubectl create secret tls foobar-tls \
  --cert cert-file --key key-file
```
Ingress with SSL:

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
      - foo.bar.com
    secretName: foobar-tls
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        backend:
          serviceName: foo-service
          servicePort: 80
      - path: /bar
        backend:
          serviceName: bat-service
          servicePort: 80
```
- https://kubernetes.io/docs/concepts/services-networking/ingress/

## **Protect node metadata and endpoints**

Restricting cloud metadata API access
Cloud platforms (AWS, Azure, GCE, etc.) often expose metadata services locally to instances. By default these APIs are accessible by pods running on an instance and can contain cloud credentials for that node, or provisioning data such as kubelet credentials. These credentials can be used to escalate within the cluster or to other cloud services under the same account.

When running Kubernetes on a cloud platform limit permissions given to instance credentials, use network policies to restrict pod access to the metadata API, and avoid using provisioning data to deliver secrets.

- https://cloud.google.com/kubernetes-engine/docs/how-to/protecting-cluster-metadata
- https://docs.projectcalico.org/security/kubernetes-nodes

# **Minimize use of, and access to, GUI elements**

# **Verify platform binaries before deploying**

https://github.com/kubernetes/kubernetes/releases

sha256sum kubernetes.tar.gz
