Ansible Dynamic Assignments (Include) and Community Roles

Create a New Branch in Your GitHub Repository
Open your terminal on the Jenkins-Ansible server or your local system (whichever you're using).
Clone your repository if not already cloned:
bash
Copy code
git clone https://github.com/<your-name>/ansible-config-mgt.git
Replace <your-name> with your GitHub username.

Navigate into the cloned directory:
bash
Copy code
cd ansible-config-mgt

I my case I already have repository in my local everthing so what I just did was to git pull origin main
to make sure my repositiry is up to date
(screenshot)

Create a new branch called dynamic-assignments:
bash
Copy code
git branch dynamic-assignments
Switch to the new branch:
bash
Copy code
git switch dynamic-assignments

(screenshot)

Create the Required Folder and File
Inside your repository, create a new folder called dynamic-assignments:
bash
Copy code
mkdir dynamic-assignments
Navigate into the dynamic-assignments folder:
bash
Copy code
cd dynamic-assignments
Create a new file named env-vars.yml:
bash
Copy code
touch env-vars.yml

(screenshot)

Update Your GitHub Repository Structure
Ensure your repository has the following structure:

Inventory Folder: Contains environment files (dev, stage, uat, prod).
Playbooks Folder: Contains site.yml.
Static Assignments Folder: Contains common.yml.
Optionally, add the roles folder for future tasks.
Run these commands to structure the repository:

bash
Copy code
mkdir -p inventory/{dev,stage,uat,prod}
mkdir -p playbooks
mkdir -p static-assignments
Add the required files:

bash
Copy code
touch playbooks/site.yml
touch static-assignments/common.yml

(screenshot)

Create env-vars Folder and Files
Navigate back to the root of your repository:
bash
Copy code
cd ..
Create a new folder named env-vars:
bash
Copy code
mkdir env-vars
Create YAML files for each environment:
bash
Copy code
touch env-vars/dev.yml env-vars/stage.yml env-vars/uat.yml env-vars/prod.yml
(screenshot)

Populate the env-vars.yml File
Open the dynamic-assignments/env-vars.yml file in an editor (e.g., nano, vim, or VS Code).
Paste the following code:
yaml
Copy code
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
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always

(screenshot)

Update the site.yml File
Open playbooks/site.yml in your editor.
Add the following content:
yaml
Copy code
---
- hosts: all
  name: Include dynamic variables
  tasks:
    import_playbook: ../static-assignments/common.yml
    include: ../dynamic-assignments/env-vars.yml
    tags:
      - always

- hosts: webservers
  name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml

(screenshot)

Verify Your Work
Check your repository on GitHub to confirm the following structure:
csharp
Copy code
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml

(screenshot)

now push your work to github from the terminal
(screenshot)
go to github and you will automatically see a compare and pull request
(screenshot)
then merge the pull request
(screenshot)


**MySQL and Load Balancer roles**

---

## **Phase 1: Prepare Your GitHub Repository**
1. **Check if Git is Installed**  
   Run this command to confirm Git is installed:  
   ```bash
   git --version
   ```
   If it’s not installed, install it with:  
   ```bash
   sudo apt update && sudo apt install git -y
   ```
(screenshot)
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
(screenshot)
---

## **Phase 2: Add MySQL Role**
1. **Download the MySQL Role**  
   Use Ansible Galaxy to download the MySQL role developed by `geerlingguy`:
   ```bash
   cd roles
   ansible-galaxy install geerlingguy.mysql
   ```
(screenshot)
2. **Rename the Folder**  
   Rename the downloaded folder to `mysql`:  
   ```bash
   mv geerlingguy.mysql/ mysql
   ```
(screenshot)

Review the Role Documentation
Inside the mysql folder, you will find a README.md file. This file contains instructions about how to use the role, including:

What variables you need to configure.
Examples of how to use the role in playbooks.
Specific features or requirements.
Steps:
Open the README.md file to understand how to configure the role:
bash
Copy code
vi roles/mysql/README.md
Look for:
Variables related to MySQL credentials (username, password, database name).
How to define these variables in your playbooks
(screenshot)

3. **Update Role Configuration**  
   Open the `roles/mysql/defaults/main.yml` file and set the correct credentials 
   for your MySQL database:
   (screenshot)
   ```yaml
mysql_databases:
  - name: tooling

mysql_users:
  - name: tooling_user
    password: tooling_password
    priv: "tooling.*:ALL"

   ```
(screenshot)
(screenshot)

---

## ** Add Load Balancer Roles**
1. **Create Nginx and Apache Roles**  
   In the `roles` directory, decide whether to use community roles or create your own. To create them:
   ```bash
   ansible-galaxy init nginx
   ansible-galaxy init apache
   ```
(sreenshot)
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

3. **Configure `loadbalancers.yml`**  
   In `static-assignments/loadbalancers.yml`, define the load balancer roles:
   ```yaml
   - hosts: lb
     roles:
       - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
       - { role: apache, when: enable_apache_lb and load_balancer_is_required }
   ```

4. **Update `site.yml`**  
   Include the load balancer assignment in `site.yml`:
   ```yaml
   - name: Loadbalancers assignment
     hosts: lb
     tasks:
       - import_playbook: ../static-assignments/loadbalancers.yml
         when: load_balancer_is_required
   ```
(sreenshot)
5. **Set Environment Variables**  
   Update `env-vars/uat.yml` to activate the desired load balancer. For example, to enable Nginx:
   ```yaml
   enable_nginx_lb: true
   load_balancer_is_required: true
   ```
(screenshot)
(screenshot)
   To switch to Apache, update the same file with the following content:
   ```yaml
   enable_nginx_lb: false
   enable_apache_lb: true
   load_balancer_is_required: true
   ```

---

## **Phase 4: Test Your Setup**
1. **Run Ansible Commands**  
   Test the configurations in the respective environments:
   ```bash
   ansible-playbook -i inventory/uat.ini playbooks/site.yml
   ```
   (screenshot)
   (screenshot)
   (screenshot)

   if you encounter an error that indicates that your Ansible controller is unable to connect to the target host  via SSH due to an issue with the SSH key. like the below screenshot
   (screenshot)
then Add the SSH Key to the Agent
Use the correct path to add the key to the SSH agent:
bash
Copy code
ssh-add /home/ubuntu/lampproject.pem
If the SSH agent is not running, start it:
bash
Copy code
eval $(ssh-agent -s)
ssh-add /home/ubuntu/lampproject.pem

and your ansible command should now run.

2. **Verify Changes**  
   Confirm that the MySQL database, Nginx, or Apache is configured based on the environment variables.

---

## **Phase 5: Finalize and Push Changes**
1. **Commit and Push Changes**  
   Commit all changes to the feature branch:
   ```bash
   git add .
   git commit -m "Added MySQL and Load Balancer roles"
   git push
   ```
(screenshot)
2. **Create a Pull Request**  
   On GitHub, create a Pull Request from the `roles-feature` branch to the `main` branch. After review, merge the changes.
(screenshot)
(screenshot)
(screenshot)
---

