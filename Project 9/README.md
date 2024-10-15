![alt text](<images/VMware Partners With Docker, Pivotal And Google To Bring Container Support To Its Platform _ TechCrunch.jpeg>)

# The Story of Life Before Docker

Before Docker, deploying applications was cumbersome and often frustrating. Developers faced challenges when transitioning from development to production environments due to inconsistent configurations. Let's imagine a scenario:



![alt text](images/teamates.jpeg)


#### **The Developer's Struggle:**
shade is a developer who just finished writing a Python web application. It works perfectly on her machine, but as soon as she hands it off to the operations team to deploy on the production server, problems begin to arise.


![alt text](images/shade.jpeg)


- **Dependency Hell:** The operations team doesn’t have the exact version of Python or other dependencies needed to run the app. After hours of debugging and configuring, they finally match Sarah's environment, but at the cost of valuable time.
- **Environment Inconsistency:** Sarah’s app used libraries that behave differently on her Mac compared to the Linux servers in production.
- **Scalability Issues:** When the app grows and multiple servers need to run it, each server must be carefully configured to ensure compatibility, which is error-prone and costly.

#### **The Virtual Machine Era:**
To solve these issues, virtual machines (VMs) were introduced. VMs allow developers to package their applications and the entire operating system together, which ensures consistency. However, VMs are heavyweight:
- **Large size:** VMs contain entire operating systems, which consume significant amounts of CPU, RAM, and storage.
- **Slow startup:** Because of the large size, booting up VMs can take several minutes.
- **Inefficiency:** Running multiple VMs on a single server consumes a lot of resources.

Shade's team struggled with these limitations. They needed something faster, more efficient, and easier to manage. Enter **Docker**.

---

### What is Docker?

Docker is a containerization platform that simplifies how applications are built, shared, and run across environments. Containers solve the very problems Shade and her team faced:
- **Lightweight:** Unlike VMs, containers share the host operating system’s kernel, making them significantly smaller and faster.
- **Consistency:** The environment inside a Docker container is identical across development, testing, and production. You can run the same container on your laptop, in the cloud, or anywhere else without worrying about compatibility issues.
- **Portability:** Containers can run on any platform that supports Docker, making applications truly portable.

In essence, Docker allows you to package your app and its dependencies into a small, isolated unit known as a **container**.

---

### How Docker Works

Docker uses a **client-server** architecture:
1. **Docker Client:** The CLI tool you use to interact with Docker.
2. **Docker Daemon:** The background service (or server) that manages images, containers, networks, and volumes.
3. **Docker Images:** A read-only blueprint for creating containers. Think of an image as the recipe and the container as the dish.
4. **Docker Containers:** These are the running instances of images. A container is like a lightweight, isolated environment where your app runs.

---

### Docker Images vs Containers

- **Docker Image:** A static file containing the code, dependencies, and runtime for your app. Images are built from Dockerfiles, which we'll explore shortly.
- **Docker Container:** A container is a live, running instance of an image. You can have multiple containers running from the same image, each operating independently.

### Dockerfile: Building Your First Image

## Step 1 Provision an instance

- Spin up and Ubuntu 24.04 t2.micro 

- SSH into the instance 



### Step 2 - Install and start docker


For **Ubuntu**:

1. Update your package index:
   ```bash
   sudo apt update
   ```

2. Install Docker:
   ```bash
   sudo apt install docker.io -y
   ```

3. Start Docker:

   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
4. Check the Status of Docker
   ```bash
   sudo systemctl status docker
   ```

5. ctrl + c to exit the running docker


### Steps 3. Clone the Docker Project

Once Docker is installed and set up, clone the project repository.

1. Install Git (if not installed):
   ```bash
   
   sudo apt install git -y 
   ```

2. Clone the project repository:
   ```bash
   git clone https://github.com/TobiOlajumoke/docker-flask
   cd docker-flask
   ```



### Step 4. **Run the Docker Application**

1. **Build the Docker Image**:

   ```bash
   docker build -t flask-application:1.0.0 .
   ```
![alt text](<images/docker build.png>)

- Check if the image built

```bash
sudo docker images
```
![alt text](<images/docker images.png>)

2.  **Run the Docker Container**:
   ```bash
docker run -d -p 8000:8000 flask-application:1.0.0
   ```

Check if the container is running if it is PROCEED to step 3

```bash
sudo docker ps
```
![alt text](<images/docker run and ps.png>)
# if the container isn't running check the list of all conatiners

```bash
sudo docker ps -a
```

to troubleshoot or findout why the cdontainer "exited" and isn't running you'll check the container logs by running this command 

```bash
sudo docker logs <container_id_or_name>

```

3.  Test in Browser
Now, go to your browser and access your EC2 public IP to check if the app is running properly:

```bash
http://<your-ec2-public-ip>:8000
```
![alt text](<images/without port 8000.png>)

4. The webpage will not work, WHY?

 we have not added the port 8000 to our security group of our instance so do thatand try accessing  the EC2 public IP

![alt text](<images/adD port 800 to SG.png>)


![alt text](<images/EC2 public IP.png>)

You have successfully deployed the Dockerized Flask app on an AWS EC2 instance. This is a common workflow in modern cloud infrastructure where applications are containerized for ease of deployment, scalability, and management.

# Let's push the image to docker hub

---

### Pushing Docker Images to Docker Hub

After successfully building and running your Docker image, you may want to share it with others or deploy it to different environments. Docker Hub is a cloud-based registry service that allows you to store and distribute Docker images.

#### **Step 1: Create a Docker Hub Account**

1. Go to [Docker Hub](https://hub.docker.com/).
2. Sign up for a free account if you don’t have one already.
![alt text](<images/hub docker.png>)
3. create a repo
![alt text](<images/create a repo.png>)
![alt text](<images/create a repo2.png>)
#### **Step 2: Log In to Docker Hub from Your Terminal**

Use the Docker CLI to log in to your Docker Hub account:

```bash
sudo docker login
```
![alt text](<images/docker login.png>)

You will be prompted to enter your Docker Hub username and password.

#### **Step 3: Tag Your Image**

Before pushing the image, you need to tag it with your Docker Hub username and a repository name. The tagging format is:

```
<your-dockerhub-username>/<repository-name>:<tag>
```

For example:

```bash
sudo docker tag flask-application:1.0.0 yourusername/flask-application:1.0.0
```
![alt text](<images/docker tag.png>)


**Why Do We Tag Docker Images?**
- **Version Control**: Tagging helps you manage different versions of your images. By using tags, you can easily identify specific versions of your application, which is crucial for testing and deployment.
- **Clarity**: Tags provide clarity about what an image contains. For example, a tag can indicate whether the image is a stable release, a beta version, or a development build.
- **Rollback**: If an issue arises with a new version, you can quickly revert to a previous, stable version using its tag.


#### **Step 4: Push the Image to Docker Hub**

Once your image is tagged, you can push it to Docker Hub using the following command:

```bash
docker push yourusername/flask-application:1.0.0
```
![alt text](<images/docker push.png>)

#### **Step 5: Verify the Push**

After the push completes, you can verify that your image is on Docker Hub by visiting your Docker Hub profile and checking the repositories.
![alt text](<images/push sucessful.png>)
![alt text](<images/push sucessful tag.png>)
---

### Why Push to Docker Hub?

- **Collaboration**: Team members can easily access shared images without having to build them from scratch.
- **Backup**: Storing images on Docker Hub acts as a backup, ensuring that you can recover or roll back to previous versions if needed.
- **Deployment**: You can pull images directly from Docker Hub in different environments, simplifying the deployment process.

---


# Terminate your ec2 instance after

GOOD JOB

![](https://media.tenor.com/xHg7HK_ziuoAAAAM/clapping-leonardo-dicaprio.gif)















