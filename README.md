# WeatherApp Deployment Pipeline

This project demonstrates a complete **CI/CD pipeline** for deploying a Flask-based weather application using **Docker, Jenkins, and Ansible**. The application fetches weather data from the OpenWeatherMap API and stores it in a **SQLite database**.

---

## 📌 Table of Contents
1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Setup Instructions](#setup-instructions)
4. [Pipeline Workflow](#pipeline-workflow)
5. [Usage](#usage)
6. [Screenshots](#screenshots)
7. [Contributing](#contributing)
8. [Acknowledgments](#acknowledgments)

---

## 🌍 Project Overview

**WeatherApp** is a simple **Flask** web application that:
- Fetches weather data for a given city using the **OpenWeatherMap API**.
- Stores the fetched data in a **SQLite database**.
- Provides **RESTful endpoints** to interact with the stored data.

### 🚀 CI/CD Pipeline Stages
1. Clone the repository from GitHub.
2. Build and push a **Docker image** to Docker Hub.
3. Deploy the application to **Vagrant-managed machines** using Ansible.

---

## ⚙️ Prerequisites

Ensure you have the following installed on your machine:
- **Git**: For version control.
- **Docker**: To containerize the application.
- **Vagrant & VirtualBox**: To create virtual machines for deployment.
- **Ansible**: For automating the deployment process.
- **Jenkins**: To manage the CI/CD pipeline.

Additionally, you will need:
- A **GitHub account** for hosting the repository.
- A **Docker Hub account** for storing the Docker image.
- An **OpenWeatherMap API key** for fetching weather data.

---

## 🛠️ Setup Instructions

### 1️⃣ Clone the Repository
```bash
git clone https://github.com/amiraelkazzaz/WeatherApp.git
cd WeatherApp
```

### 2️⃣ Install Dependencies
```bash
pip install -r requirements.txt
```

### 3️⃣ Configure OpenWeatherMap API Key
Replace the placeholder `your_openweathermap_api_key` in **app.py** with your actual API key:
```python
API_KEY = "your_openweathermap_api_key"
```

### 4️⃣ Build and Test Locally
```bash
docker build -t weather-app .
docker run -p 5000:5000 weather-app
```
Access the application at:  
🔗 **http://localhost:5000**

### 5️⃣ Push the Docker Image to Docker Hub
```bash
docker tag weather-app your-dockerhub-username/weather-app:v1
docker push your-dockerhub-username/weather-app:v1
```

### 6️⃣ Set Up Vagrant Machines

Create a `Vagrantfile` and configure two virtual machines:
```ruby
Vagrant.configure("2") do |config|
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/focal64"
    web1.vm.network "private_network", ip: "192.168.56.101"
  end

  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/focal64"
    web2.vm.network "private_network", ip: "192.168.56.102"
  end
end
```
Start the machines:
```bash
vagrant up
```

### 7️⃣ Configure Jenkins
- Install **Jenkins** and required plugins (**Git, Docker Pipeline, Ansible**).
- Add credentials for **GitHub** and **Docker Hub**.
- Create a **new pipeline job** and configure it to use the `Jenkinsfile`.

---

## 🔄 Pipeline Workflow

### 1️⃣ Clone Repository
   - Pulls the latest code from **GitHub**.

### 2️⃣ Build Docker Image
   - Builds a **Docker image** using the `Dockerfile`.

### 3️⃣ Push Docker Image to Docker Hub
   - Logs in to **Docker Hub** and pushes the image with a unique tag.

### 4️⃣ Deploy with Ansible
   - Runs an **Ansible playbook** to deploy the application on Vagrant-managed machines.
   - Installs **Docker**, pulls the image, and runs the container.

---

## 📌 Usage

Once the pipeline completes successfully, the application will be deployed on the Vagrant machines.  
Access it via:
- 🔗 **http://192.168.56.15:5000/**
- 🔗 **http://192.168.56.14:5000/**

### 🖥️ Available Endpoints:
- `/weather` → Fetch weather data.
- `/cities` → Retrieve stored city weather data.

---

## 🖼️ Screenshots

| Application from first machine  | Application from second  machine  | Jenkins Pipeline |
|-------------------------------- | --------------------------------- |------------------|
|![App](images/192.168.56.14:5000..png)|![App](images/192.168.56.15:5000.png) | ![Jenkins](images/jenkins.png) |

---

## 🙌 Acknowledgments

This project was built using:
- **Flask**
- **Docker**
- **Jenkins**
- **Ansible**
- **OpenWeatherMap API**
```

