# Deploying applications on OpenShift


### **1. Deploying Prebuilt Container Images**

If you already have a container image in a registry (like Docker Hub or Quay), you can deploy it directly using `oc new-app`.

#### **1.1. Deploy from a Public Docker Image**
```bash
oc new-app <image-name> --name=<app-name>
```
Example:
```bash
oc new-app nginx --name=my-nginx-app
```

### **2. Exposing Applications to External Traffic**

Once your application is deployed, you’ll need to expose it so that it can be accessed from outside the cluster.

#### **2.1. Expose the Service as a Route**
```bash
oc expose svc/<service-name>
```
Example:
```bash
oc expose svc/my-node-app
```

This will create a public URL for your service using the default OpenShift router.

#### **2.2. Check the Route URL**
```bash
oc get routes
```
You can then visit the public URL generated in your browser.

---

### **3. Scaling Applications**

You can easily scale your applications in OpenShift by increasing or decreasing the number of running pods.

#### **3.1. Scale a Deployment**
```bash
oc scale deployment/<deployment-name> --replicas=<number>
```
Example:
```bash
oc scale deployment/my-node-app --replicas=3
```

#### **3.2. Auto-Scaling an Application**
You can also configure auto-scaling for your application based on CPU utilization:

```bash
oc autoscale deployment/<deployment-name> --min=<min-pods> --max=<max-pods> --cpu-percent=<cpu-threshold>
```
Example:
```bash
oc autoscale deployment/my-node-app --min=2 --max=10 --cpu-percent=80
```


### **4 Monitoring and Logging**

#### **4.1. View Pod Logs**
```bash
oc logs <pod-name>
```
Example:
```bash
oc logs my-node-app-1-abcde
```

#### **4.2. Access Application Metrics**
To view resource usage (CPU, memory) for your app's pods:

```bash
oc adm top pods -n <project-name>
```

---

### **5. Deleting Applications**

When you're done with an application, you can clean up resources by deleting the deployment, service, and other related resources.

#### **5.1. Delete an Application**
```bash
oc delete all -l app=<app-name>
```
Example:
```bash
oc delete all -l app=my-node-app
```

This deletes all resources (pods, services, routes, etc.) related to the application.
