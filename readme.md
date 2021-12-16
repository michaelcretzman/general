# Requirements
- Harness: 
  - Docker Desktop minimum version 4.3.1 (72247)
    - 4 GB memory and 2 CPUs
    - Docker Compose (included in Docker Desktop)
    - Kubernetes is installed and running (a new installation of Docker Desktop might need to have Kubernetes enabled in its **Settings**).
- Quickstart:
  - Github account.
  - Minikube minimum version v1.22.0 installed locally.

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

# CD Pipeline Quickstart
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
17. The Git Connector settings appear. Enter the following settings.
- **Name:** enter a name for the Connector.
- **URL Type:** select Repository.
- **Connection Type:** select HTTP.
- **Git Repository URL:** enter https://github.com/kubernetes/website.
- Username and password: Enter the username and password for your Github account. You'll have to create a Harness secret for the password.
18. Click **Continue**.
19. In **Set Up Delegates**, click **Install new Delegate**. The Delegate wizard appears.
20. Click **Kubernetes**, and then click **Continue**.
21. Enter a name for the Delegate, like **quickstart**, click the **Laptop** size.
22. Click Continue.
23. Click Download Script. The YAML file for the Kubernetes Delegate will download to your computer as an archive.
24. Open a terminal and navigate to where the Delegate file is downloaded.
25. Start minkube if it is not already started: `minikube start`. We'll use the `default` namespace for our deployment, but you can use another existing namespace.
26. Next, install the Harness Delegate using the `harness-delegate.yaml` file you just downloaded. In the terminal running minikube, run this command (you can see this command in the Delegate wizard):

```
kubectl apply -f harness-delegate.yaml
```
The successful output is something like this:
```
% kubectl apply -f harness-delegate.yaml
namespace/harness-delegate unchanged
clusterrolebinding.rbac.authorization.k8s.io/harness-delegate-cluster-admin unchanged
secret/k8s-quickstart-proxy unchanged
statefulset.apps/k8s-quickstart-sngxpn created
service/delegate-service unchanged
```
27. In Harness, click **Verify**. It will take a few minutes to verify the Delegate. Once it is verified, close the wizard.
28. Back in **Set Up Delegates**, you can select the new Delegate.
29. In the list of Delegates, select the **Connect using Delegates with the following Tags** option.
30. Enter the tag of the new Delegate and click **Save and Continue**. When you are done, the Connector is tested. 
31. Click **Continue**.
32. In **Manifest Details**, enter the following settings:

- **Manifest Identifier:** enter nginx.
- **Git Fetch Type:** select **Latest from Branch**.
- Branch: enter main.
- File/Folder path: `content/en/examples/application/nginx-app.yaml` This is the path from the repo root.
33. Test the connection and click **Submit**. The manifest is now listed.
34. Click **Next** at the bottom of the **Service** tab. Now that the artifact and manifest are defined, you can define the target cluster for your deployment.
35. In **Infrastructure Details**, in **Specify your environment**, click **New Environment**.
36. In **New Environment**, enter a name such as **quickstart**, select **Non-Production**, and click **Save**. The new Environment appears.
37. In **Infrastructure Definition**, click **Kubernetes**.
38. 
