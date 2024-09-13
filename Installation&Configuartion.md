# Jenkins Installation Guide

## **1. Jenkins Installation on Windows**

### **Step 1: Download Jenkins**

- Download the Windows `.msi` installer from the official Jenkins website:
  ```bash
  https://jenkins.io/download/
  ```

### **Step 2: Install Jenkins**

1. Run the `.msi` installer.
2. Follow the installation wizard and choose the installation path.
3. During installation, Jenkins requires **Java**. Ensure that you have **Java 8** or **Java 11** installed.
4. Once installed, Jenkins will start as a **Windows Service**.

### **Step 3: Configure Jenkins**

- Jenkins will be available at `http://localhost:8080` after installation.
- Get the initial password:
  - Navigate to the location where Jenkins is installed:
    ```bash
    C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword
    ```
  - Copy the password, paste it on the Jenkins setup page, and continue with the installation.

---

## **2. Jenkins Installation on Linux (Ubuntu)**

### **Step 1: Install Java**

Jenkins requires **Java** to run. Install OpenJDK using the following commands:

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version   # To verify installation
```

### **Step 2: Add Jenkins Repository**

1. Add Jenkins GPG key:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
   /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   ```

2. Add the Jenkins repository:
   ```bash
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
   https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```

### **Step 3: Install Jenkins**

1. Update the package list:
   ```bash
   sudo apt update
   ```

2. Install Jenkins:
   ```bash
   sudo apt install jenkins -y
   ```

### **Step 4: Start Jenkins**

After installation, start Jenkins and ensure it is running:

```bash
sudo systemctl start jenkins
sudo systemctl status jenkins
```

### **Step 5: Access Jenkins**

- Jenkins will be running on port `8080`. To access it, open a browser and go to:
  ```bash
  http://<your_server_ip>:8080
  ```

- To get the initial admin password, run the following command:
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```

---

## **3. Jenkins Security Configuration**

### **Step 1: Install Role-Based Strategy Plugin**
- Install the **Role-Based Strategy** plugin from the Jenkins Plugin Manager.
- Enable this plugin under **Configure Global Security**.

### **Step 2: Create Users**
- Go to **Manage Jenkins** → **Manage Users**.
- Create two users, `user1` and `user2`.

### **Step 3: Manage and Assign Roles**
- **Manage Roles**:
  - Define a **Global Role** (e.g., *company*) and add an employee group with read-only permissions for all users.
  - Define **Item Roles** for developers (e.g., *developer group*) with the pattern `dev.*` (regular expressions). For example, for the job `dev_production`, give access to developers only.
- **Node Role**:
  - Define the **Master-Slave Node Role** for node management.

### **Step 4: Assign Roles**
- In **Global Roles**, assign `user1` and `user2` to the **employee** group.
- In **Item Roles**, assign:
  - `user1` to the **developer** group (can access dev jobs only).
  - `user2` to the **tester** group (can access test jobs only).

### **Step 5: Verify Role Assignment**
- Log in as `user1` and `user2` to verify that they can only access their assigned jobs.

---

## **4. Jenkins-Tomcat Integration**

### **Step 1: Install Apache Tomcat**

- **Download Tomcat**: Get the `.tar.gz` file from the official website.
- Use `wget` to download and extract the file:
   ```bash
   wget <tomcat-link>
   tar -xvzf apache-tarfile.tar.gz
   mv apache-tomcat /opt/
   ```

- **Change Jenkins Port Number** to avoid conflict with Tomcat:
  ```bash
  vi /etc/default/jenkins
  # Change port from 8080 to any other available port
  service jenkins restart
  ```

### **Step 2: Deploy a WAR File**

1. Download a sample Java WAR file using `wget`:
   ```bash
   wget <java-war-file-link>
   ```

2. Copy the WAR file to Jenkins workspace:
   ```bash
   cd /var/lib/jenkins/workspaces/autodeployment
   cp /path/to/sample.war .
   ```

3. **Install Tomcat Deployment Plugin**: Install **Deploy to Container** plugin in Jenkins.

### **Step 3: Post Build Actions**
- Configure **Post Build Actions**:
   - Add **Tomcat Deployment**.
   - Specify the `sample.war` file to deploy.
   - Define the **context path** (e.g., `http://<server-ip>:8080/sample`).

---

## **5. Jenkins Task - CLI, Notifications, and Decryption**

### **Step 1: Jenkins Version Using CLI**
- Use the Jenkins CLI to get the version of Jenkins:
  ```bash
  java -jar jenkins-cli.jar -s http://localhost:8080/ version
  ```

### **Step 2: Email Notifications**
- Configure email notifications by installing the **Email Extension Plugin**.
- Search for detailed instructions on YouTube for better understanding.

### **Step 3: Find and Decrypt Deployer Password**
- Locate and decrypt Jenkins deployer password hashes:
  - Jenkins stores credentials in an encrypted form. Look for the hashed password in the Jenkins configuration files.
  - Search for solutions in **StackOverflow** if you encounter issues.

## Feel free to give it a  STAR