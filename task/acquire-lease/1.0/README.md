An implementation of leases to add concurrency to tekton based on Holly Cummins excellent work in this blog [article](https://medium.com/ibm-garage/using-lease-resources-to-manage-concurrency-in-tekton-builds-344ba84df297).

This task, when paired with the release-lease task, provides a mutex that can be used to manage concurrency of the pipeline.

To acquire a lease in your pipeline, use the following as the first task in the pipeline:

```
- name: acquire-lease
    taskRef:
    name: acquire-lease
    kind: ClusterTask
    params:
    - name: lease-name
        value: "$(context.pipeline.name)"
    - name: owner
        value: "$(context.pipelineRun.name)"
```

The lease name will be set to the name of the pipeline and the owner reference in the spec will be set to the pipelinerun name using the above. Note the lease-name is used as the mutex.

To release the lease, add a finally block to your pipeline with the release-lease cluster task:

```
  finally:
    - name: release-lease
      taskRef:
        name: release-lease
        kind: ClusterTask
      params:
        - name: lease-name
          value: "$(context.pipeline.name)"
```