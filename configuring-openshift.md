# Configuring an OpenShift cluster

### 1. **General Cluster Information**

#### **Get Cluster Status**
```bash
oc status
```
Displays the current status of the cluster and any critical warnings.

#### **Get Cluster Operators**
```bash
oc get clusteroperators
```
Shows the health of all cluster operators, indicating whether they are available, progressing, or degraded.

#### **Check Cluster Version**
```bash
oc get clusterversion
```
Provides information on the current OpenShift version installed on the cluster.

### 2. **Node Management**

#### **List All Nodes**
```bash
oc get nodes
```
Lists all the nodes in the OpenShift cluster, including their roles (e.g., master, worker).

#### **Describe Node**
```bash
oc describe node <node-name>
```
Displays detailed information about a specific node, including its labels, allocated resources, and status.


### 3. **Project (Namespace) Management**

#### **Create a New Project**
```bash
oc new-project <project-name>
```
Creates a new project (namespace) for organizing workloads.

#### **Delete a Project**
```bash
oc delete project <project-name>
```
Deletes a project and all associated resources.

#### **Switch to a Project**
```bash
oc project <project-name>
```


