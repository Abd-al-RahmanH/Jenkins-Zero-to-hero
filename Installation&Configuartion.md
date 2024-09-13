# Jenkins Installation Guide by Abdul Rahman H

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
