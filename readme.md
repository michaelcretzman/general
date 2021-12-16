# Requirements
Docker: 
- Minimum Supported Version: 4.3.1
- 4 GB memory and 2 CPUs
- Docker Compose
- Kubernetes is installed and running (a new installation of Docker Desktop might need to have Kubernetes enabled in Settings.

# Architecture Summary

Harness Community Edition has two main components:

-   **Harness Manager** — Harness Manager is where your CD configurations are stored and your pipelines are managed. After you install Harness, you sign up in the Manager at http://localhost/#/signup.
    Pipeline are triggered manually in the Harness Manager or automatically in response to Git events, schedules, new artifacts, and so on.  
    Harness Manager is available either as SaaS (running in the Harness cloud) or as On-Prem (running in your infrastructure).
-   **Harness Delegate** — The Harness Delegate is a software service you install in your environment that connects to the Harness Manager and performs tasks using your container orchestration platforms, artifact repositories, etc.
   You can install a Delegate inline when setting up connections to your resources or separately as needed. This guide will walk you through setting up a Harness Delegate inline.

# Installation

 1. Make sure your system meets the **Requirements** above.
 2. Clone the Harness Git repo into your local directory:
 ```
 git clone git@github.com:harness/harness-cd-community.git
```
 
 3. Change directory to the `harness` folder:
```
cd harness-cd-community/docker-compose/harness
```
 4. Build and run Harness using Docker Compose:
```
docker-compose up -d
```
**Note:** Depending on your Internet connection speed, the first time you download Harness can take 3–12 mins (download images and start all containers). You won't be able to sign up until all the required containers are up and running.
 5. Run the following command to start the Harness Manager:
 ```
 docker-compose run --rm proxy wait-for-it.sh ng-manager:7090 -t 180
 ```
 6. Wait until you see that `ng-manager` is available:
 ```
 wait-for-it.sh: ng-manager:7090 is available after 66 seconds

 ```
 7. In browser, go to the URL http://localhost/#/signup.
 8. Enter an email address and password and click **Sign up**.

You're now using Harness!
The next section walks you through setting up and running a simple CD Pipeline using a public manifest and Docker image.

# Create and Run a CD Pipeline

