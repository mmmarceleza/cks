# Service Accounts

## Video

| Portuguese |
| :--------: |

| [![portuguese](https://img.youtube.com/vi/xxxxxxxx/hqdefault.jpg)](https://youtu.be/xxxxxxxx) |

## Concept

In Kubernetes, a service account is an identity used by pods to access the Kubernetes API or other resources in the cluster. It is used to authenticate and authorize the actions performed by the pods within the cluster.

Key points about service accounts in Kubernetes:

- Pod Identity: Every pod in Kubernetes is associated with a service account, which determines the permissions and access level of that pod.

- Default Service Account: If a pod does not explicitly specify a service account, it is automatically assigned the default service account in its namespace. The default service account usually has minimal privileges.

- Role-Based Access Control (RBAC): Service accounts are often used in conjunction with RBAC to define granular permissions for pods. RBAC rules are defined to specify what actions a service account is allowed to perform. I am going to explore this topic in a future section.

- Secrets: When a pod is associated with a service account, a token and a set of secret can be created and mounted into the pod. These credentials can be used by the pod to authenticate with the Kubernetes API server.

- Use Cases: Service accounts are commonly used in scenarios where pods need to interact with the Kubernetes API server, for example, to list or watch resources, create new resources, or update existing ones.

- Service Account Objects: Service accounts are represented by Kubernetes objects of type ServiceAccount. You can create and manage service accounts using kubectl or by defining them in YAML manifests.

Here is an example of a service account YAML manifest:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-service-account
```

## Connecting in the API with Service Account
