# Debug Operations in Kubernetes

The Kubernetes `kubectl` command-line interface allows you to interact with and manage your Kubernetes cluster. This section contains `kubectl` commands that aid with debugging your Pods, including listing Pod details, examining logs, executing container commands, and creating dedicated debugging resources.

For a full list of `kubectl` commands, visit the official [Kubectl Reference Docs](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).


## List Pod Details

To list details for the Pods in your cluster, issue the `get pods` command. `get pods` returns details such as a Pod's status, age, and health, allowing you to identify potential issues.

**Example**:

The following command returns details for Pods in the current namespace. Include the `--namespace=NAMESPACE` flag to review details about Pods in a specific namespace, or `--all-namespaces` to retrieve information on all Pods across all namespaces.

```shell
kubectl get pods 
```

```shell
NAME                                READY     STATUS    RESTARTS   AGE
nginx-deployment-1006230814-6winp   1/1       Running   0          7m
nginx-deployment-1006230814-fmgu3   1/1       Running   0          7m
nginx-deployment-1370807587-6ekbw   1/1       Running   0          1m
nginx-deployment-1370807587-fg172   0/1       Pending   0          1m
nginx-deployment-1370807587-fz9sd   0/1       Pending   0          1m
```


## Retrieve Logs

Container logs allow you to monitor and debug cluster activity and performance. To print logs, issue the `kubectl logs` command with the name of your Pod and a specific container. 

**Example**:

The following command prints logs for container `container1` in Pod `mypod`. If your Pod only has one container, you can omit the `-c CONTAINER` flag. To retrieve the logs from all the containers in the Pod, include the `--all-containers` flag.

```shell
kubectl logs mypod -c container1
```

```shell
0: Fri Apr  1 11:42:23 UTC 2022
1: Fri Apr  1 11:42:24 UTC 2022
2: Fri Apr  1 11:42:25 UTC 2022
```


## Execute a Command in a Container
You can issue a command inside a specific container with `kubectl exec`. This command is useful if your container image includes debugging utilities. 

**Example**:

The following example issues `exec` with the Pod `mypod` and the `cat` command to retrieve authentication logs from a Linux-based container image.

```shell
kubectl exec mypod -- cat /var/log/auth.log
```

## Create a Dedicated Debugging Resource
If a container has crashed or does not include an image with debugging tools, you can use `kubectl debug` to create a dedicated debugging resource with additional debugging capabilities and configurations. 

`kubectl debug` provides two main ways of creating debugging resources: add a temporary "ephemeral" container to an existing Pod, or make a copy of a Pod.

### Add an Ephemeral Container
You can launch a temporary ephemeral container in an existing Pod to add additional debugging tools that may not be available in your current container image. 

**Example**:

The following command adds an ephemeral busybox container to `mypod`. 

```shell
kubectl debug my-ephemeral-pod --it --image=busybox --target=my-ephemeral-pod
```

```text
Defaulting debug container name to debugger-8xzrl.
If you do not see a command prompt, try pressing enter.
/ #
```

### Copy an Existing Pod
When you copy an existing Pod for debugging purposes, you can choose to add a new container, change the container command, or change the container image.

For example, if your problematic Pod does not provide access to a shell, you can create a copy of the Pod and add an appropriate container image. 

**Example**:

The following command makes a copy of `mypod` called `mypod-debug` and adds an Ubuntu container image.

```shell
kubectl debug mypod -it --image=ubuntu --copy-to=mypod-debug
```


```text
Defaulting debug container name to debugger-6odjr.
If you do not see a command prompt, try pressing enter.
/ #
```

For a comprehensive review of `debug` options, visit the [debug](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#debug) section of the Kubernetes Resource Docs.


# References

- [Kubectl Reference Docs](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

- [What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/)
