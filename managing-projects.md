# Managing projects in OpenShift

### **1. Creating and Managing Projects**

#### **1.1. Create a New Project** 
To create a new project (namespace) where applications will be deployed, use:

```bash
oc new-project <project-name> --description="<description>" --display-name="<display-name>"
```

Example:
```bash
oc new-project ag-project --description="This is a test project" --display-name="My Test Project"
```

- **Project Name**: The actual name of the project (required).
- **Description**: A brief description of the project (optional).
- **Display Name**: The name displayed in the OpenShift console UI (optional).

#### **1.2. List All Projects**
To view all projects in the cluster:

```bash
oc get projects
```

You can also list projects with specific details using:

```bash
oc get projects -o wide
```

#### **1.3. Get Details of a Specific Project**
To get detailed information about a specific project:

```bash
oc describe project <project-name>
```

Example:
```bash
oc describe project ag-project
```

#### **1.4. Switch Between Projects**
Switch to another project to set the context for your subsequent commands:

```bash
oc project <project-name>
```

Example:
```bash
oc project ag-project
```

This will switch your working context to `ag-project`.

---

### **2. Managing Project Resources**

#### **2.1. View Resources in a Project**
You can view all resources (pods, services, routes, etc.) in a project using:

```bash
oc get all -n <project-name>
```

Example:
```bash
oc get all -n ag-project
```
This will list all resources in the specified project.

#### **2.2. Delete a Project**
To delete a project and all its associated resources (pods, services, routes, etc.):

```bash
oc delete project <project-name>
```

Example:
```bash
#oc delete project ag-project
```


### **3. Managing Users and Permissions in a Project**

#### **3.1. Assign Users to a Role in a Project**
To give a user access to a project, you can assign them a role, such as `admin`, `edit`, or `view`. Use the following command to assign a role to a user:

```bash
oc adm policy add-role-to-user <role> <username> -n <project-name>
```

Example:
```bash
oc adm policy add-role-to-user admin user1 -n ag-project
```

- **admin**: Full control over the project.
- **edit**: Can modify resources but not grant permissions.
- **view**: Read-only access to the project.

#### **3.2. Remove a User from a Role**
To remove a user from a role in a project:

```bash
oc adm policy remove-role-from-user <role> <username> -n <project-name>
```

Example:
```bash
oc adm policy remove-role-from-user admin user1 -n ag-project
```

#### **3.3. List All Users and Their Roles in a Project**
To view all roles and their associated users within a project:

```bash
oc get rolebindings -n <project-name>
```


Example:
```bash
oc get rolebindings -n ag-project
```


This will display which users or groups have which roles in the project.


### **5. Monitoring Projects**

#### **5.2. View Event Logs for a Project**
To view events happening in a project (like pod creation, errors, etc.):

```bash
oc get events -n <project-name>
```

---

### **7. Managing Pods in a Project**

#### **7.1. View All Pods in a Project**
To list all pods running in a project:

```bash
oc get pods -n <project-name>
```

#### **7.2. Delete a Pod**
To delete a specific pod in a project:

```bash
oc delete pod <pod-name> -n <project-name>
```

Example:
```bash
oc delete pod my-app-pod-1234 -n ag-project
```

#### **7.3. Restart a Pod**
To restart a pod, you can delete it, and OpenShift will automatically recreate it if it's part of a deployment or replication controller:

```bash
oc delete pod <pod-name> -n <project-name>
```

---

### **8. Deleting Projects and Cleaning Up Resources**

#### **8.1. Delete All Resources in a Project**
To delete all resources (pods, services, routes, etc.) in a project without deleting the project itself:

```bash
oc delete all --all -n <project-name>
```

#### **8.2. Delete the Project**
To delete a project and all its associated resources:

```bash
oc delete project <project-name>
```
