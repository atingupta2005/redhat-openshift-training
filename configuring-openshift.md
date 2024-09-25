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

#### **Cordoning and Draining a Node**
To mark a node unschedulable (cordon it):
```bash
oc adm cordon <node-name>
```

To drain a node (move workloads off of it):
```bash
oc adm drain <node-name> --ignore-daemonsets --delete-emptydir-data
```

#### **Uncordon a Node**
To make the node schedulable again:
```bash
oc adm uncordon <node-name>
```

### 3. **User and Role Management**

#### **Create a New User (HTPasswd Authentication)**
```bash
htpasswd -b users.htpasswd <username> <password>
oc create secret generic htpass-secret --from-file=htpasswd=users.htpasswd -n openshift-config
```

#### **Assign a Role to a User**
Assign a cluster role (e.g., `admin`) to a user:
```bash
oc adm policy add-cluster-role-to-user <role> <username>
```

To assign a project-specific role (e.g., `edit`) to a user in a project:
```bash
oc adm policy add-role-to-user <role> <username> -n <project-name>
```

#### **Remove a User from a Role**
```bash
oc adm policy remove-cluster-role-from-user <role> <username>
```

### 4. **Project (Namespace) Management**

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


