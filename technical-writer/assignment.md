# Debug Operations in Kubernetes

The Kubernetes `kubectl` command-line tool communicates with the Kubernetes API to perform tasks such as retrieving Pod information, printing logs, and executing container commands. In this topic, we'll cover commands that can aid you in debugging your Kubernetes clusters.

For a list of all `kubectl` commands, see the offical [Kubectl Reference Docs](https://kubernetes.io/docs/reference/kubectl/quick-reference/).


## List Pod Details

To list details for the Pods in your cluster, use the `get pods` command. `get pods` returns details such as a Pod's running status, age, and health, allowing you to identify potential issues.

```shell
kubectl get pods 
```

**Sample output**:
```shell
NAME                                READY     STATUS    RESTARTS   AGE
nginx-deployment-1006230814-6winp   1/1       Running   0          7m
nginx-deployment-1006230814-fmgu3   1/1       Running   0          7m
nginx-deployment-1370807587-6ekbw   1/1       Running   0          1m
nginx-deployment-1370807587-fg172   0/1       Pending   0          1m
nginx-deployment-1370807587-fz9sd   0/1       Pending   0          1m
```

By default, `get pods` returns details for Pods in your current namespace. Include the `--namespace=NAMESPACE` flag to review details about Pods in a specific namespace, or `--all-namespaces` to retrieve information on all Pods across all namespaces.


## Retrieve Logs

Container logs allow you to monitor and debug cluster activity and performance. To print logs, issue the `kubectl logs` command with the name of your Pod and a specific container. If your Pod only has one container, you can omit the `-c CONTAINER` flag.


```shell
kubectl logs mypod -c container1
```

**Sample output**:
```shell
0: Fri Apr  1 11:42:23 UTC 2022
1: Fri Apr  1 11:42:24 UTC 2022
2: Fri Apr  1 11:42:25 UTC 2022
```

To retrieve the logs from all the containers in the Pod, use the `--all-containers` flag.


## Execute a Command in a Container
You can run a command inside a specific container with `kubectl exec`. This command is useful if your container image includes debugging utilities. For example, to review authentication logs from a Linux-based container image, issue `exec` with the Pod name and `cat` command.

```shell
kubectl exec mypod -- cat /var/log/auth.log
```

## Debug
If a container has crashed or does not include an image with debugging capabilities, you can use `kubectl debug` to create a copy of the Pod with certain attributes changed. 

For example, you can configure the copy to not terminate if an error is experienced inside the container. 




# References

- [Kubectl Reference Docs](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

- [What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/)
