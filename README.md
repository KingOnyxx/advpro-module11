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

---
# Reflection 2
1. What is the difference between Rolling Update and Recreate deployment strategy?
- Rolling Update: This strategy updates the pods in a deployment one by one, ensuring that the application remains available throughout the update process. It gradually replaces the old pods with new ones, allowing the application to continue serving requests without downtime.
- Recreate: This strategy terminates all the old pods at once and creates new ones in their place. This results in a brief downtime during the update process, as the application is unavailable while the new pods are being created.
2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt.
i created a new patch file called `recreate.yaml` with the following content
``` yaml
spec:
  strategy:
    $retainKeys:
    - type
    type: Recreate
```
then i applied the patch using the following command
``` shell
kubectl patch deployments spring-petclinic-rest --patch-file ./recreate.yaml
```
3. Prepare different manifest files for executing Recreate deployment strategy.
``` shell
kubectl get deployments/spring-petclinic-rest -o yaml > deployment-recreate.yaml
```
4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking kubectl apply -f command) to the cluster.
- Using Kubernetes manifest files provides a declarative way to define the desired state of the application and its environment. This allows for easy version control, sharing, and reproducibility of the deployment configuration.
- When deploying the app manually, we need to remember the exact commands and options to run, which can be error-prone and time-consuming. By using manifest files, we can define the deployment configuration once and apply it consistently across different environments.
- Applying the manifest files to the cluster using `kubectl apply -f` is a more efficient and reliable way to deploy the application, as it ensures that the desired state is achieved without manual intervention. This reduces the risk of human error and streamlines the deployment process.