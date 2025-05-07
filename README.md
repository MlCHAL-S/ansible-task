# README

## Instructions

1. **Create Ansible Inventory**
   - Create an Ansible inventory file with information about your hosts.

2. **Create Ansible Playbook for Spring-Petclinic Application**
   - The playbook should include the following:
     - Installing required software depending on the application artifact (Java-based or Docker-based).
     - Taking a specific artifact version (JAR file or Docker image) from a local machine or Nexus and deploying it to the application server.

3. **Deploy Application to Remote Host with Ansible**
   - Use the Ansible playbook to automatically deploy the Spring-Petclinic application to the specified remote host.

4. **Check Results in Browser**
   - After deployment, verify that the application is running by accessing it through the browser.

---

## Solution:

### Prepare Repo and VM
Open GCP console and terminal:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/ansible -C “$(whoami)” -N ""
```

```bash
chmod 400 ~/.ssh/ansible
```

```bash
cat ~/.ssh/ansible.pub
```

and paste it into VM’s settings.


```bash
sudo apt update && sudo apt install ansible -y
```

```bash
git clone https://github.com/MlCHAL-S/spring-clinic-cicd.git && cd spring-clinic-cicd
```

```bash
docker build -t pet-clinic-image .
```

Create Artifact Repo manually:

```bash
docker tag pet-clinic-image:latest us-central1-docker.pkg.dev/$(gcloud config get-value project)/pet-clinic-repo/pet-clinic-image:latest
```

```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

Then push with:

```bash
docker push us-central1-docker.pkg.dev/$(gcloud config get-value project)/pet-clinic-repo/pet-clinic-image:latest
```

## Ansible Task

```bash
git clone https://github.com/MlCHAL-S/ansible-task.git && cd ansible-task
```


Remember to change:
- ansible.cfg: set remote_user
- inventory/ip: set host IP
- deploy-pet-clinic.yml: set project

Run the playbook:

```bash
ansible-playbook deploy-app.yml
```

Useful links:
https://docs.docker.com/engine/install/debian/#install-from-a-package