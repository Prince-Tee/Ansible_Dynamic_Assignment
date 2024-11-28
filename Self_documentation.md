# Self-Study Documentation for Ansible Dynamic Assignments and Community Roles

This documentation serves as a guide for troubleshooting common issues encountered while working with Ansible dynamic assignments and community roles, along with detailed steps for setting up a GitHub repository and configuring roles.

## Common Problems and Solutions

### **1. Problem: Git Repository Not Up-to-Date**
- **Issue**: The repository was not updated with the latest changes from the main branch.  
- **Solution**: Ran the command 
  ```bash
  git pull origin main
  ``` 
  to fetch and merge the latest changes from the main branch into the local repository.

---

### **2. Problem: Ansible SSH Connection Failure**
- **Issue**: Ansible was unable to connect to the target host due to an SSH key error.  
- **Solution**:  
  1. Added the SSH key to the agent using the command:
     ```bash
     ssh-add /home/ubuntu/lampproject.pem
     ```
  2. If the SSH agent was not running, started it using:
     ```bash
     eval $(ssh-agent -s)
     ssh-add /home/ubuntu/lampproject.pem
     ```

---

### **3. Problem: Errors with Repository Structure**
- **Issue**: The directory structure was incomplete or missing essential files.  
- **Solution**:  
  1. Created the necessary folders using:
     ```bash
     mkdir -p inventory/{dev,stage,uat,prod}
     mkdir -p playbooks
     mkdir -p static-assignments
     ```
  2. Added missing files with:
     ```bash
     touch playbooks/site.yml
     touch static-assignments/common.yml
     ```

---

### **4. Problem: Configuration of Dynamic Variables in Ansible**
- **Issue**: Dynamic environment-specific variables were not properly included in the playbooks.  
- **Solution**:  
  - Created a `dynamic-assignments/env-vars.yml` file and added the appropriate logic to include environment-specific files.

---

### **5. Problem: Issues with Load Balancer Role Configuration**
- **Issue**: There was confusion about how to configure and enable the load balancer roles (Nginx or Apache).  
- **Solution**:  
  - Created roles for Nginx and Apache using:
    ```bash
    ansible-galaxy init nginx
    ansible-galaxy init apache
    ```
  - Added default variables in their respective `defaults/main.yml` files to control whether the roles were enabled.

---

### **6. Problem: Testing Errors in Ansible Playbooks**
- **Issue**: Errors occurred while running the Ansible playbooks due to improper configuration or missing SSH setup.  
- **Solution**:  
  1. Ensured environment variables were correctly defined in the `env-vars` files.  
  2. Fixed SSH key issues as outlined in Problem #2.  
  3. Ran the playbooks with:
     ```bash
     ansible-playbook -i inventory/uat.ini playbooks/site.yml
     ```

---

### **7. Problem: Problems with MySQL Role Variables**
- **Issue**: Variables for the MySQL role were not set correctly, which caused configuration issues.  
- **Solution**:  
  - Edited `roles/mysql/defaults/main.yml` to include proper MySQL user credentials and database configurations:
    ```yaml
    mysql_databases:
      - name: tooling

    mysql_users:
      - name: tooling_user
        password: tooling_password
        priv: "tooling.*:ALL"
    ```

---

### **8. Problem: Difficulty Understanding Community Roles**
- **Issue**: Unfamiliarity with how to use community roles downloaded from Ansible Galaxy.  
- **Solution**:  
  - Reviewed the `README.md` file in the `roles/mysql` directory for usage instructions.

---

### **9. Problem: Pushing Changes to GitHub**
- **Issue**: Needed to push local changes to the GitHub repository.  
- **Solution**:  
  - Committed changes and pushed them to a new branch:
    ```bash
    git add .
    git commit -m "Added MySQL and Load Balancer roles"
    git push
    ```

---

## Setting Up Your GitHub Repository

### Create a New Branch in Your GitHub Repository

1. Open your terminal on the Jenkins-Ansible server or your local system.
2. Clone your repository if not already cloned:
   ```bash
   git clone https://github.com/<your-name>/ansible-config-mgt.git
   ```
   Replace `<your-name>` with your GitHub username.
3. Navigate into the cloned directory:
   ```bash
   cd ansible-config-mgt
   ```
   If you already have the repository locally, run:
   ```bash
   git pull origin main
   ```
4. Create a new branch called dynamic-assignments:
   ```bash
   git branch dynamic-assignments
   ```
5. Switch to the new branch:
   ```bash
   git switch dynamic-assignments
   ```

### Create Required Folders and Files

1. Inside your repository, create a new folder called dynamic-assignments:
   ```bash
   mkdir dynamic-assignments
   ```
2. Navigate into the dynamic-assignments folder:
   ```bash
   cd dynamic-assignments
   ```
3. Create a new file named env-vars.yml:
   ```bash
   touch env-vars.yml
   ```

### Update Your GitHub Repository Structure

Ensure your repository has the following structure:

```
Inventory Folder: Contains environment files (dev, stage, uat, prod).
Playbooks Folder: Contains site.yml.
Static Assignments Folder: Contains common.yml.
```

Run these commands to structure the repository:
```bash
mkdir -p inventory/{dev,stage,uat,prod}
mkdir -p playbooks
mkdir -p static-assignments
```
Add the required files:
```bash
touch playbooks/site.yml
touch static-assignments/common.yml
```

### Create env-vars Folder and Files

1. Navigate back to the root of your repository:
   ```bash
   cd ..
   ```
2. Create a new folder named env-vars:
   ```bash
   mkdir env-vars
   ```
3. Create YAML files for each environment:
   ```bash
   touch env-vars/dev.yml env-vars/stage.yml env-vars/uat.yml env-vars/prod.yml 
   ```

### Populate the env-vars.yml File

Open the `dynamic-assignments/env-vars.yml` file in an editor (e.g., nano, vim, or VS Code) and paste in appropriate code.

### Update site.yml File

Open `playbooks/site.yml` in your editor and add necessary content.

### Verify Your Work

Check your repository on GitHub to confirm that it has been structured correctly.

### Finalize and Push Changes

1. Commit all changes to your feature branch.
2. Create a Pull Request on GitHub from your feature branch to merge changes into main after review.

This self-study documentation should assist you in effectively managing Ansible configurations while troubleshooting common issues encountered during setup and execution of tasks within your projects.