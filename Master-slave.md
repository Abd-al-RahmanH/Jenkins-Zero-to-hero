# Jenkins Master-Slave Configuration

## Prerequisites

- **Master:** Jenkins installed with Java (`jenkins-java`)
- **Slave:** Java installed with the agent (`java --agent`)

## RSA Key Generation and Setup

### Master Configuration

1. **Generate RSA Keys:**
   - Access the terminal as the Ubuntu user, not with `sudo` or `su -` commands.
   - Navigate to the `.ssh` directory:
     ```bash
     cd ~/.ssh
     ```
   - Create RSA key files:
     ```bash
     touch id_rsa id_rsa.pub
     ```
   - **Do not use `sudo` or `su` for these commands.**

2. **Configure Authorized Keys:**
   - Copy the public key to the `authorized_keys` file:
     ```bash
     cat id_rsa.pub >> authorized_keys
     ```
   - Set the correct permissions:
     ```bash
     chmod 600 authorized_keys
     ```

3. **Set Private Key:**
   - Copy the private key content to the `id_rsa` file:
     ```bash
     vi id_rsa
     ```
   - Set the correct permissions:
     ```bash
     chmod 400 id_rsa
     ```

4. **File Transfer:**
   - If the slave needs to send a file to the master, generate and use RSA keys to encrypt the file before sending.

5. **Generate Keys on Slave (GUI Option):**
   - Optionally, you can generate public and private keys on the slave machine through the GUI.

### Slave Machine Setup

1. **Create EC2 Instance:**
   - Launch an EC2 instance with the same OS as the master, using the same private key, and ensure port 22 is open.

2. **Install Java:**
   - Add the repository and install Java:
     ```bash
     sudo apt-get update
     sudo apt-get install openjdk-11-jdk
     ```

### Jenkins Master Configuration

1. **Add Slave Node:**
   - Go to Jenkins as an admin:
     ```
     Manage Jenkins -> Manage Nodes and Clouds
     ```
   - Add a new node:
     - **Name:** Slave 1
     - **Number of Executors:** 2
     - **Remote Working Directory:** `/path/to/jen`
     - **Launch Method:** Launch via SSH (using the SSH Build Agent plugin)
     - **Host:** Slave IP
     - **Credentials:** SSH username with private key (e.g., user: `ubuntu`, paste the private key)
     - **Verification Strategy:** Manually trusted key verification strategy
   - Save the configuration.

2. **Verify Node Connection:**
   - After saving, connect to the slave node. You might see `removing.jar` files in the `/path/to/jen` directory.

3. **Create and Configure Jobs:**
   - Create a job for the slave, restrict access, and apply the label `sbi`. Check the `/path/to/jen` folder.

4. **Adjust Idle Timeout:**
   - Change idle timeout settings according to the available RAM (e.g., 5 or 10 minutes).

### Plugin Installation

1. **Delivery Pipeline Plugin:** For project views used by managers.
2. **Build Pipeline Plugin:** For developers, add labels.
3. **Build Monitor Plugin:** To view builds.
4. **Task Pipeline Plugin:** Most commonly used in projects.
5. **Parameterized Jobs Plugin:** For handling input/output in jobs.

### Additional Information

- **Connecting to EC2 if Private Key is Lost:**
  - Master (Ubuntu) can connect to Slave (Amazon Linux) using a new key, then replace the lost key.

- **Declarative vs Scripted Pipelines:**
  - Declarative Pipelines provide a simpler syntax with a more structured approach.
  - Scripted Pipelines offer more flexibility but are more complex.

- **Parameterized Trigger Plugin:**
  - Handles inputs and outputs between jobs. The job runs based on provided inputs and generates outputs accordingly.

Feel free to adjust the paths and commands according to your specific setup. 
