# CIS Benchmark

## Videos

| English | Portuguese |
|----------|:-------------:|
| [![english](https://i.ytimg.com/vi/7NlPes7lz1Y/hqdefault.jpg)](https://youtu.be/7NlPes7lz1Y) | [![portuguese](https://img.youtube.com/vi/ncnFCvfdrxY/hqdefault.jpg)](https://youtu.be/ncnFCvfdrxY)

## What is CIS Benchmark?

The CIS Benchmark (Center for Internet Security Benchmark) is a set of guidelines and best practices designed to enhance the security of various computer systems, software applications, and infrastructure components. These benchmarks are developed by the Center for Internet Security, a nonprofit organization focused on improving cybersecurity through the creation and promotion of security standards and best practices.

The CIS Benchmark provides a detailed, step-by-step guide on how to configure and secure a specific technology or platform to meet security standards and reduce vulnerabilities. These benchmarks are created through a collaborative effort involving experts from various industries, including cybersecurity professionals, vendors, and government agencies.

When it comes to Kubernetes, CIS has developed the CIS Kubernetes Benchmark, which is a set of security recommendations and configuration guidelines specifically tailored for Kubernetes clusters. 

Here's why you need to use the CIS Benchmark for Kubernetes:

- Security Assurance: Kubernetes is a complex system with many configuration options, and misconfigurations can lead to security vulnerabilities. The CIS Kubernetes Benchmark helps you implement security best practices to reduce the risk of security breaches.

- Compliance Requirements: Many organizations must comply with regulatory requirements and industry standards related to security. The CIS Benchmark for Kubernetes can serve as a valuable resource to help organizations align with these requirements.

- Reduced Vulnerabilities: By following the guidelines outlined in the CIS Kubernetes Benchmark, you can reduce common vulnerabilities associated with Kubernetes, such as unauthorized access, insecure container configurations, and exposed sensitive information.

- Hardening: The benchmark provides hardening recommendations, which are measures aimed at increasing the overall security posture of your Kubernetes clusters. These measures help protect your applications and data from potential threats.

- Best Practices: The benchmark includes best practices for securing various aspects of Kubernetes, such as API server security, pod security, network policies, and more. Implementing these best practices can improve the overall security of your Kubernetes environment.

- Continuous Improvement: Security is an ongoing process. The CIS Benchmark is periodically updated to reflect changes in the threat landscape and the evolving Kubernetes ecosystem. Regularly reviewing and implementing updates from the benchmark helps you stay ahead of emerging threats.

Here are a few examples of tools that can be used to implement the CIS Kubernetes Benchmark:

- [CIS-CAT Pro](https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro)
- [kube-bench](https://github.com/aquasecurity/kube-bench)
- [trivy](https://aquasecurity.github.io/trivy)
- [trivy-operator](https://aquasecurity.github.io/trivy-operator/latest)

In summary, the CIS Benchmark for Kubernetes is a valuable resource for organizations looking to secure their Kubernetes clusters effectively. It provides detailed guidance and best practices to reduce security risks, comply with industry standards, and ensure that your Kubernetes environment is configured securely and resilient against potential threats.

## Exploring Some Tools

### CIS-CAT Pro

CIS-CAT Pro, short for Center for Internet Security Configuration Assessment Tool Pro, is a powerful and comprehensive software tool developed by the Center for Internet Security (CIS) to assist organizations in assessing the security of their systems and networks against the CIS Benchmarks and other security standards. You need to become a CIS SecureSuite Member to have access to this tool.

### Kube-bench

kube-bench is a tool that checks whether Kubernetes is deployed securely by running the checks documented in the CIS Kubernetes Benchmark. Tests are configured with YAML files, making this tool easy to update as test specifications evolve.

Basically, there are two ways to use this tool. In the first one, you need to download and run the binary on one node of the cluster. In the second one, you can create jobs to execute the scan. 

Here are some examples of how to use this tool:

#### On the node

Ubuntu/Debian:

```
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.6.2/kube-bench_0.6.2_linux_amd64.deb -o kube-bench_0.6.2_linux_amd64.deb

sudo apt install ./kube-bench_0.6.2_linux_amd64.deb -f
```

RHEL:

```
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.6.2/kube-bench_0.6.2_linux_amd64.rpm -o kube-bench_0.6.2_linux_amd64.rpm

sudo yum install kube-bench_0.6.2_linux_amd64.rpm -y
```

Alternatively, you can manually download and extract the kube-bench binary:

```
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.6.2/kube-bench_0.6.2_linux_amd64.tar.gz -o kube-bench_0.6.2_linux_amd64.tar.gz

tar -xvf kube-bench_0.6.2_linux_amd64.tar.gz
```
> [!NOTE]
> **Remember to check the release version on Github to download the latest version.**

You can then run kube-bench directly:

```
kube-bench
```

If you manually downloaded the kube-bench binary (using curl command above), you have to specify the location of the configuration directory and file. For example:

```
./kube-bench --config-dir `pwd`/cfg --config `pwd`/cfg/config.yaml
```


#### Kubernetes Vanilla

```
kubectl apply -f job.yaml
```

Inspect the log of the created Pod.

#### AKS

```
kubectl apply -f job-aks.yaml
```

Inspect the log of the created Pod.


#### EKS

```
kubectl apply -f job-eks.yaml
```

Inspect the log of the created Pod.

#### GKE

```
kubectl apply -f job-gke.yaml
```

Inspect the log of the created Pod.

### Trivy

[Trivy](https://github.com/aquasecurity/trivy) is a famous open-source tool for scanning many targets. It is possible to use it to check the good practices proposed by the CIS Benchmark Kubernetes. There are many ways to install trivy, depending on your distribution. Follow the instructions on the [documentation page](https://aquasecurity.github.io/trivy/v0.45/getting-started/installation/).

Here are some examples:

Summary table:
```
trivy k8s cluster --compliance=k8s-cis --report summary
```

Detailed table:
```
trivy k8s cluster --compliance=k8s-cis --report all
```

### Trivy-Operator

Trivy has a native Kubernetes Operator that continuously scans your Kubernetes cluster for security issues and generates security reports as Kubernetes Custom Resources. It does it by watching Kubernetes for state changes and automatically triggering scans in response to changes, for example, initiating a vulnerability scan when a new Pod is created. There are three ways to install, according to the documentation:

- [kubectl](https://aquasecurity.github.io/trivy-operator/v0.16.1/getting-started/installation/kubectl/)
- [helm](https://aquasecurity.github.io/trivy-operator/v0.16.1/getting-started/installation/helm/)
- [Operator Lifecycle Manager](https://aquasecurity.github.io/trivy-operator/v0.16.1/getting-started/installation/olm/)

Choose the one who attends to your needs.

After installing the software, the first scan starts automatically. Wait the scan Pods concluded their jobs and look for the objects called `clustercompliancereport`.

```
kubectl get clustercompliancereports
```

Inspect the one with the name `cis`:

```
kubectl get clustercompliancereports cis -o yaml
```

The ClusterComplianceReport is a cluster-scoped resource that represents the latest compliance control check results. The report spec defines a mapping between pre-defined compliance control check ids to security scanners check ids.

## References

- [CIS Benchmark kubernetes](https://www.cisecurity.org/benchmark/kubernetes)
- [CIS Benchmark kubernetes GKE](https://cloud.google.com/kubernetes-engine/docs/concepts/cis-benchmarks)
- [CIS Benchmark kubernetes AKS](https://learn.microsoft.com/en-us/azure/aks/cis-kubernetes)
- [CIS Benchmark kubernetes EKS](https://aws.amazon.com/blogs/containers/introducing-cis-amazon-eks-benchmark/)
- [Kube-bench](https://github.com/aquasecurity/kube-bench)
- [Trivy Kubernetes Compliance Reports](https://aquasecurity.github.io/trivy/v0.45/docs/target/kubernetes/#compliance)
- [Trivy Operator Kubernetes ClusterComplianceReport](https://aquasecurity.github.io/trivy-operator/v0.16.1/docs/crds/clustercompliance-report/)
