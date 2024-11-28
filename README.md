# Ansible Dynamic Assignments and Community Roles Documentation

## Create a New Branch in Your GitHub Repository

1. **Open your terminal** on the Jenkins-Ansible server or your local system (whichever you're using).
2. **Clone your repository** if not already cloned:
   ```bash
   git clone https://github.com/<your-name>/ansible-config-mgt.git
   ```
   Replace `<your-name>` with your GitHub username.
3. **Navigate into the cloned directory**:
   ```bash
   cd ansible-config-mgt
   ```
   In my case, I already have the repository in my local environment, so what I just did was to:
   ```bash
   git pull origin main
   ```
   to make sure my repository is up to date 
   ![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/cd%20into%20your%20repository%20and%20git%20pull.PNG)
4. **Create a new branch called dynamic-assignments**:
   ```bash
   git branch dynamic-assignments
   ```
5. **Switch to the new branch**:
   ```bash
   git switch dynamic-assignments
   ```
   ![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/create%20a%20branch%20and%20switch%20to%20it.PNG)

## Create the Required Folder and File

1. Inside your repository, **create a new folder called dynamic-assignments**:
   ```bash
   mkdir dynamic-assignments
   ```
2. **Navigate into the dynamic-assignments folder**:
   ```bash
   cd dynamic-assignments
   ```
3. **Create a new file named env-vars.yml**:
   ```bash
   touch env-vars.yml
   ```
   ![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/create%20the%20dynamic%20assignment%20and%20env-vars.png)

## Update Your GitHub Repository Structure

Ensure your repository has the following structure:

- **Inventory Folder**: Contains environment files (dev, stage, uat, prod).
- **Playbooks Folder**: Contains site.yml.
- **Static Assignments Folder**: Contains common.yml.
- Optionally, add the roles folder for future tasks.

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
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/ensure%20your%20repository%20has%20the%20required%20structure.PNG)

## Create env-vars Folder and Files

1. Navigate back to the root of your repository:
   ```bash
   cd ..
   ```
2. **Create a new folder named env-vars**:
   ```bash
   mkdir env-vars
   ```
3. **Create YAML files for each environment**:
   ```bash
   touch env-vars/dev.yml env-vars/stage.yml env-vars/uat.yml env-vars/prod.yml
   ```
   ![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/Create%20env-vars%20Folder%20and%20Files.PNG)

## Populate the env-vars.yml File

1. Open the `dynamic-assignments/env-vars.yml` file in an editor (e.g., nano, vim, or VS Code).
2. Paste the following code:
    ```yaml
    ---
    - name: collate variables from env specific file, if it exists
      hosts: all
      tasks:
        - name: looping through list of available files
          include_vars: "{{ item }}"
          with_first_found:
            - files:
              - dev.yml
              - stage.yml
              - prod.yml
              - uat.yml
            paths: "{{ playbook_dir }}/../env-vars"
          tags: always
    ```
    ![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/Populate%20the%20env-vars.yml%20File.PNG)

## Update the site.yml File

1. Open `playbooks/site.yml` in your editor.
2. Add the following content:
    ```yaml
    ---
    - hosts: all
      name: Include dynamic variables
      tasks:
        import_playbook: ../static-assignments/common.yml
        include: ../dynamic-assignments/env-vars.yml

    - hosts: webservers
      name: Webserver assignment
      import_playbook: ../static-assignments/webservers.yml 
    ```
    ![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/Update%20the%20site.yml%20File.PNG)

## Verify Your Work

Check your repository on GitHub to confirm the following structure:
```
├── dynamic-assignments 
│     └── env-vars.yml 
├── env-vars 
│     └── dev.yml 
│     └── stage.yml 
│     └── uat.yml 
│     └── prod.yml 
├── inventory 
│     └── dev 
│     └── stage 
│     └── uat 
│     └── prod 
├── playbooks 
│     └── site.yml 
└── static-assignments 
      └── common.yml 
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/confirm%20the%20following%20structure.PNG)

Now push your work to GitHub from the terminal 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/push%20dynamic%20assignments%20branch%20to%20github.PNG)

Go to GitHub and you will automatically see a compare and pull request 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/compare%20and%20pull%20request%20on%20github.PNG)

then merge the pull request 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/dynamic-assignments%20pull%20request%20succesful.PNG)

---

## MySQL and Load Balancer Roles

### Phase 1: Prepare Your GitHub Repository

1. **Check if Git is Installed**
   
Run this command to confirm Git is installed:
```bash
git --version 
```
If it’s not installed, install it with:
```bash
sudo apt update && sudo apt install git -y 
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/checking%20if%20git%20is%20installed.PNG)

2. **Initialize Git in Your Directory**

Navigate to the `ansible-config-mgt` directory:
```bash 
cd ansible-config-mgt 
```
Initialize Git:
```bash 
git init 
```

3. **Connect to Your GitHub Repository**

Pull your repository from GitHub (replace `<your-name>` with your GitHub username):
```bash 
git pull https://github.com/<your-name>/ansible-config-mgt.git 
```
Add the remote origin:
```bash 
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git 
```

4. **Create a New Feature Branch**

Create and switch to a new branch for roles:
```bash 
git branch roles-feature 
git switch roles-feature 
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/creating%20the%20switch%20role%20feature%20branch.PNG)

---

### Phase 2: Add MySQL Role

1. **Download the MySQL Role**

Use Ansible Galaxy to download the MySQL role developed by `geerlingguy`:
```bash 
cd roles 
ansible-galaxy install geerlingguy.mysql 
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/downloading%20ansible-galaxy%20for%20geerlingguy%20mysql.PNG)

2. **Rename the Folder**

Rename the downloaded folder to `mysql`:
```bash 
mv geerlingguy.mysql/ mysql 
```
![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/rename%20the%20ansible-galaxy%20geerlingguy%20mysql%20folder%20with%20this%20command.PNG)

3. **Review the Role Documentation**

Inside the mysql folder, you will find a README.md file. This file contains instructions about how to use the role, including:

- What variables you need to configure.
- Examples of how to use the role in playbooks.
- Specific features or requirements.

Steps: Open the README.md file to understand how to configure the role:
```bash 
vi roles/mysql/README.md 
```
Look for:

- Variables related to MySQL credentials (username, password, database name).
- How to define these variables in your playbooks 

![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/reading%20the%20README%20fle.PNG)

4. **Update Role Configuration**

Open the `roles/mysql/defaults/main.yml` file and set the correct credentials for your MySQL database:
```yaml
mysql_databases:
  - name: tooling
  
mysql_users:
  - name: tooling_user
    password: tooling_password
    priv: "tooling.*:ALL"
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/content%20of%20the%20default%20main%20yml%20file.PNG) 

![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/editing%20th%20database%20and%20user.PNG)

![screenshot](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/all%20the%20first%20commsnd%20for%20the%20second%20phase.PNG)

---

### Phase 3: Add Load Balancer Roles

1. **Create Nginx and Apache Roles**

In the `roles` directory, decide whether to use community roles or create your own. To create them:
```bash 
ansible-galaxy init nginx 
ansible-galaxy init apache 
```
![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/commands%20for%20adding%20loadbalancers%20yml%20site%20yml.PNG)

2. **Add Default Variables**

In `roles/nginx/defaults/main.yml`, add:
```yaml
enable_nginx_lb: false  
load_balancer_is_required: false  
```

In `roles/apache/defaults/main.yml`, add:
```yaml  
enable_apache_lb: false  
load_balancer_is_required: false  
```
![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/commands%20for%20adding%20loadbalancers%20yml%20site%20yml.PNG)

3. **Configure loadbalancers.yml**

In `static-assignments/loadbalancers.yml`, define the load balancer roles:
```yaml  
- hosts: lb  
  roles:  
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }  
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }  
```

4. **Update site.yml**

Include the load balancer assignment in `site.yml`:
```yaml  
- name: Loadbalancers assignment  
  hosts: lb  
  tasks:  
    - import_playbook: ../static-assignments/loadbalancers.yml  
      when: load_balancer_is_required  
```

5. **Set Environment Variables**

Update `env-vars/uat.yml` to activate the desired load balancer. For example, to enable Nginx:
```yaml  
enable_nginx_lb: true  
load_balancer_is_required: true  
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/Edit%20env-vars%20uat%20yml%20to%20activate%20a%20load%20balancer.PNG) 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/content%20of%20env-vars%20uat%20yml%20to%20activate%20a%20load%20balancer.PNG)

To switch to Apache, update the same file with the following content:
```yaml  
enable_nginx_lb: false  
enable_apache_lb: true  
load_balancer_is_required: true  
```

---

### Phase 4: Test Your Setup

1. **Run Ansible Commands**

Test the configurations in the respective environments:
```bash  
ansible-playbook -i inventory/uat.ini playbooks/site.yml  
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/running%20ansible.png) 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/running%20ansible2.png) 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/running%20ansible3.png) 

If you encounter an error that indicates that your Ansible controller is unable to connect to the target host via SSH due to an issue with the SSH key like below screenshot 

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/error%20in%20running%20ansible.png) 

then:

**Add the SSH Key to the Agent**
Use the correct path to add the key to the SSH agent:
```bash  
ssh-add /home/ubuntu/lampproject.pem  
```
If the SSH agent is not running, start it:
```bash  
eval $(ssh-agent -s)  
ssh-add /home/ubuntu/lampproject.pem   
```
and your Ansible command should now run.

2. **Verify Changes**

Confirm that MySQL database, Nginx, or Apache is configured based on environment variables.

---

### Phase 5: Finalize and Push Changes

1. **Commit and Push Changes**

Commit all changes to the feature branch:
```bash  
git add .   
git commit -m "Added MySQL and Load Balancer roles"   
git push   
```
![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/git%20push%20git%20add%20roles%20feature%20branch.PNG)

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/git%20add%20and%20git%20push.PNG)

2. **Create a Pull Request**

On GitHub, create a Pull Request from `roles-feature` branch to `main` branch. After review, merge changes.

![(screenshot)](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/creat%20pull%20request.PNG) 

![(screenshot) ](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/create%20pull%20request%201.PNG)

![(screenshot).](https://github.com/Prince-Tee/Ansible_Dynamic_Assignment/blob/main/ScreenshotFrom%20my%20env/pull%20request%20successful.PNG)


**Conclusion**
The documentation outlines a comprehensive process for implementing dynamic assignments and community roles in Ansible, specifically focusing on creating a structured GitHub repository. It details the steps for branching, folder organization, and file creation, ensuring that users can effectively manage their Ansible configurations.

Key phases include:

1. **Repository Preparation**: Users are guided through cloning their GitHub repository, creating a new branch, and establishing a proper directory structure to accommodate various environments and playbooks.

2. **Role Implementation**: The documentation emphasizes the addition of MySQL and load balancer roles using Ansible Galaxy, along with instructions for configuring these roles to suit specific requirements.

3. **Testing and Verification**: Users are instructed on how to run Ansible commands to test their configurations and troubleshoot any SSH connection issues that may arise.

4. **Finalization**: The process concludes with committing changes, pushing them to GitHub, and creating pull requests to merge updates into the main branch.

This structured approach ensures that users can effectively manage their Ansible projects while leveraging community roles for enhanced functionality.