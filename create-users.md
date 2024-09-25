# Set Up HTPasswd and Create Users in OpenShift

---

### **1. Configure HTPasswd Authentication**

First, you need to configure OpenShift to use HTPasswd for user authentication. This allows users to authenticate with a username and password.

#### **1.1. Install `httpd-tools` to Use `htpasswd` Utility**

Install the `httpd-tools` package, which provides the `htpasswd` utility for managing user credentials:

```bash
sudo yum install httpd-tools
```

#### **1.2. Create an HTPasswd File and Add Users**

Create a new **htpasswd** file to store user credentials. If the file already exists, you can add users to it.

To create a new file with a user (e.g., `admin`):

```bash
htpasswd -c -B -b users.htpasswd admin admin_password
```

- `-c` creates a new file.
- `-B` uses bcrypt for password encryption.
- `-b` specifies the username and password inline.

For additional users (without recreating the file):

```bash
htpasswd -b users.htpasswd user1 password123
htpasswd -b users.htpasswd user2 password456
```

#### **1.3. Create an HTPasswd Secret in OpenShift**

Create a secret from the HTPasswd file that OpenShift will use for authentication:

```bash
oc create secret generic htpass-secret --from-file=htpasswd=users.htpasswd -n openshift-config
```

If you need to update the secret later, delete the old one and recreate it:

```bash
oc delete secret htpass-secret -n openshift-config
oc create secret generic htpass-secret --from-file=htpasswd=users.htpasswd -n openshift-config
```

#### **1.4. Edit the OpenShift OAuth Configuration to Use HTPasswd**

Now, configure OpenShift to use the newly created HTPasswd secret for user authentication. Edit the OAuth configuration:

```bash
oc edit oauth cluster
```

Add or modify the `identityProviders` section like this:

```yaml
identityProviders:
- name: my_htpasswd_provider
  mappingMethod: claim
  type: HTPasswd
  htpasswd:
    fileData:
      name: htpass-secret
```

Save and exit the editor. OpenShift will now use the HTPasswd file for user authentication.

---

### **2. Creating New Users via `oc` Commands**

After configuring HTPasswd authentication, you can create users using the `htpasswd` utility and manage roles via `oc`.

#### **2.1. Add New Users**

To add a new user, update the **htpasswd** file:

```bash
htpasswd -b users.htpasswd newuser newpassword
```

Then, update the secret in OpenShift:

```bash
oc delete secret htpass-secret -n openshift-config
oc create secret generic htpass-secret --from-file=htpasswd=users.htpasswd -n openshift-config
```

#### **2.2. Assign Roles to Users**

To give a user permissions, you assign them a role. For example, to give a user **cluster-admin** privileges:

```bash
oc adm policy add-cluster-role-to-user cluster-admin newuser
```

For project-level roles, such as **admin** for a specific project:

```bash
oc adm policy add-role-to-user admin newuser -n <project-name>
```

To give the user **view** access in a project:

```bash
oc adm policy add-role-to-user view newuser -n <project-name>
```

---

### **3. Creating New Users via OpenShift Web Console (Portal UI)**

#### **3.1. Login to the Web Console as Admin**

1. Open the OpenShift web console in your browser (usually at `https://<openshift-cluster-url>:8443`).
2. Log in using an admin account.

#### **3.2. Access User Management**

1. Navigate to **Administration > Users**.
2. You should see the list of current users.

#### **3.3. Add New Users via Web Console**

While OpenShift does not provide direct user creation in the UI when using HTPasswd, you can manage user roles and projects via the **Web Console**.

- Assign a role to the new user by navigating to **Projects** and assigning them a role within a project.

---


### **4. Managing Users**

If needed, you can manage users by viewing them with:

```bash
oc get users
```

To delete a user:

```bash
oc delete user <username>
```
