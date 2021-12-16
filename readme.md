# Requirements
- Docker: 
  - Docker Desktop. Minimum Supported Version: 4.3.1 (72247)
  - 4 GB memory and 2 CPUs
  - Docker Compose (included in Docker Desktop)
  - Kubernetes is installed and running (a new installation of Docker Desktop might need to have Kubernetes enabled in its **Settings**).
- Github account.

# Architecture Summary

Harness Community Edition has two main components:

-   **Harness Manager:** Harness Manager is where your CD configurations are stored and your pipelines are managed. After you install Harness, you sign up in the Manager at http://localhost/#/signup.
    Pipeline are triggered manually in the Harness Manager or automatically in response to Git events, schedules, new artifacts, and so on.  
    Harness Manager is available either as SaaS (running in the Harness cloud) or as On-Prem (running in your infrastructure).
-   **Harness Delegate:** The Harness Delegate is a software service you install in your environment that connects to the Harness Manager and performs tasks using your container orchestration platforms, artifact repositories, etc.
   You can install a Delegate inline when setting up connections to your resources or separately as needed. This guide will walk you through setting up a Harness Delegate inline.

# Installation
:clock9: 5-12m

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
**Note:** The first download can take 3–12 mins (downloading images and starting all containers). You won't be able to sign up until all the required containers are up and running.

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

You'll see a message like:

> Welcome John Smith, let's get you started!

You're now using Harness!

The next section walks you through setting up and running a simple CD Pipeline using a public manifest and Docker image.

# Create and Run a CD Pipeline
:clock9: 10m

1. In Harness, click **Projects**.
2. Click **Create a Project**.
3. In **About the Project**, in **Name**, enter **quickstart**, and then click **Save and Continue**.
4. In **Invite Collaborators**, click **Save and Continue**.
Your new project appears. Let's add a CD Pipeline.
5. Click the new project.
6. In **Modules**, click **Continuous Delivery**.
7. Click **Create a Pipeline**.
8. In **Create new Pipeline**, enter the name **quickstart**, and then click **Start**.
Your new Pipeline is started! Let's add a CD stage.
9. Click **Add Stage**.
10. In **Select stage type**, click **Deploy**.
11. In **Stage Name**, enter **deploy**, and then click **Set Up Stage**.
The new stage appears. Now we'll set up the Service, Infrastructure, and Execution for the stage.
12. In **Specify Service**, click **New Service**.
13. In **New Service**, enter the name **nginx**, and then click **Save**.
14. In **Manifests**, click **Add Manifest**.
15. Select **K8s Manifest**, and click **Continue**.
16. In **Select K8sManifest Store**, click **GitHub**, and then click **New GitHub Connector**.


