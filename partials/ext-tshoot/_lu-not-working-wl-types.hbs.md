### Symptom

When using `server` or `worker` as a
[workload type](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-workloads-workload-types.html#types),
live update might not work.

### Cause

The default pod selector that detects when a pod is ready to do Live Update is incorrectly using
the label `'serving.knative.dev/service': 'WORKLOAD-NAME'`.
This label is not present on  `server` or `worker` workloads.

### Solution

Go to the project's `Tiltfile`, look for the `k8s_resource` line, and modify the `extra_pod_selectors`
parameter to use any pod selector that matches your workload. For example:

```code
extra_pod_selectors=[{'carto.run/workload-name': 'WORKLOAD-NAME', 'app.kubernetes.io/component': 'run', 'app.kubernetes.io/part-of': 'WORKLOAD-NAME'}]
```