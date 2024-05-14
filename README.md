# Reflection 1
1. Compare the application logs before and after you exposed it as a Service.
before exposing as a service
``` shell
I0513 08:29:35.788665       1 log.go:195] Started HTTP server on port 8080
I0513 08:29:35.788871       1 log.go:195] Started UDP server on port  8081
```
after exposing as a service
``` shell
I0513 08:29:35.788665       1 log.go:195] Started HTTP server on port 8080
I0513 08:29:35.788871       1 log.go:195] Started UDP server on port  8081
I0513 08:38:36.137363       1 log.go:195] GET /
I0513 08:38:36.214969       1 log.go:195] GET /
I0513 08:40:24.647862       1 log.go:195] GET /
I0513 08:40:24.733486       1 log.go:195] GET /
```
before exposing to service the 2 lines are the logs when the server starting, and the last four lines are the output of the client accessing the server.

2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
- The `-n` option is used to specify the namespace in which the resources are to be listed. 
- The first namespace is called `default`, which is automatically set if no option is provided. This namespace is used as the default namespace for created objects. Since we didn't specify a namespace when creating the nodes and services, they were assigned to the `default` namespace.
- The second namespace is `kube-system`, which is automatically assigned to objects created by the Kubernetes system. This namespace typically contains internal system components such as `kube-dns`, `kube-proxy`, and `kubernetes-dashboard`.

