## spcn10
## Natchapon Supatsorn 
## Kubernetes In Windows

## Wakatime
https://wakatime.com/@Hgot7/projects/acwytwmpyq?start=2023-03-14&end=2023-03-20

## 1. Install Kubectl
   - Ref 
    - https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

   - download Kubectl.exe to path want

      ```
      curl.exe -LO "https://dl.k8s.io/release/v1.26.0/bin/windows/amd64/kubectl.exe"
      ``` 
   - Add Path to environment variable
      - Search environment
  
        ![image](https://user-images.githubusercontent.com/119097663/224904080-a7de4fcd-c43d-4760-b483-0734aaeca796.png)


      - Click Environment Variables...

        ![image](https://user-images.githubusercontent.com/119097663/224904504-ac4bb0b8-4a35-4ddd-87c0-d0f665c86d04.png)

       - Select Path Click Edit

        ![image](https://user-images.githubusercontent.com/117428887/226186513-dc6e3b21-491e-42c0-8167-c094c8063f5b.png)

       - Click New
        
        ![image](https://user-images.githubusercontent.com/117428887/226186676-19c0a2c1-d21b-43ac-83b8-3d97b0cf9b34.png)

      - Add Path that have kubectl.exe
      - Click OK
  
      - Test Kubectl enable in command
      ```ruby
      kubectl version --client
      ```

## 2. Install minikube
   - Ref
    - https://minikube.sigs.k8s.io/docs/start/

      - download minikube.exe

      ```ruby
      New-Item -Path 'c:<path want to install>' -Name 'minikube' -ItemType Directory -Force #create folder minikube
      Invoke-WebRequest -OutFile 'c:<path want to install>\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing #download install to path
      ```

      - Add Path to environment variable
      ```ruby
      $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
      if ($oldPath.Split(';') -inotcontains 'C:<path folder minikube.exe>'){ `
      [Environment]::SetEnvironmentVariable('Path', $('{0};C:<path folder minikube.exe>' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
      }
      ```
   - Restart Terminal

## 3. Install Docker Desktop
   - Ref
    - https://docs.docker.com/desktop/install/windows-install/

## Test minikube start
1. Start a cluster using the docker driver
   ```ruby
   minikube start --driver=docker
   ```
   ![image](https://user-images.githubusercontent.com/117428887/226186725-ad795e36-54b5-442d-bbfd-08ec2437bfb7.png)

2. Run with open dashboard
   ```ruby
   minikube dashboard
   ```
   ![minikube](https://user-images.githubusercontent.com/117428887/226186765-0f02770b-b293-4a2b-88f7-ad4ca8d842e3.png)

3. Test services
   ```ruby
   minikube service hello-minikube
   ```
   ![image](https://user-images.githubusercontent.com/119097663/224907641-f32599e8-afd0-4a9e-8bf5-f8a59c476752.png)

4. Test Stop minikube
   ```ruby
   minikube pause
   ```
   ![minikube](https://user-images.githubusercontent.com/117428887/226186765-0f02770b-b293-4a2b-88f7-ad4ca8d842e3.png)

## 4. Install traefik
1. Install Traefik Resource Definitions
   ```ruby
   kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
   ```
    ![image](https://user-images.githubusercontent.com/117428887/226186940-130aa934-f25e-4e7f-98aa-fb28a57d3518.png)

2. Install RBAC for Traefik
   ```ruby
   kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml
   ```
    ![image](https://user-images.githubusercontent.com/117428887/226187375-35e350b3-5e86-4258-8019-eab5f4457c5c.png) 

3. Install Traefik Helmchart
   ```ruby
   helm repo add traefik https://traefik.github.io/charts 
   helm repo update 
   helm install traefik traefik/traefik 
   ```
    ![image](https://user-images.githubusercontent.com/117428887/226187599-56c3f09f-3d89-4594-802b-90c3c36488e4.png)

4. Verify service is running
   ```ruby
   kubectl get svc -l app.kubernetes.io/name=traefik
   kubectl get po -l app.kubernetes.io/name=traefik
   ```
    ![image](https://user-images.githubusercontent.com/117428887/226187639-404cd269-6581-4991-a2b9-d02505eb3bac.png)

5. copy user in dashboard-secret place it at user in traefik-dashboard

   ![image](https://user-images.githubusercontent.com/119097836/226182889-ccb98c0b-15ff-4bb5-8c6c-d78d948b580e.png)

   ![image](https://user-images.githubusercontent.com/119097836/226182737-5386691f-26c0-4c30-852c-0fd31860dfb8.png)

   ![image](https://user-images.githubusercontent.com/119097836/226182939-127db28b-5e6a-48f5-a9ea-496880f5fa44.png)

   ![image](https://user-images.githubusercontent.com/119097663/226111244-a7ec1e11-8f01-4070-88ba-0c7a88f83cc1.png)

6. Deploy
   ```ruby
   kubectl apply -f . 
   ```
   ![image](https://user-images.githubusercontent.com/117428887/226187639-404cd269-6581-4991-a2b9-d02505eb3bac.png)

## Result

## 1. dashboard

![minikube](https://user-images.githubusercontent.com/117428887/226186765-0f02770b-b293-4a2b-88f7-ad4ca8d842e3.png)

## 2. https://traefik.spcn10.local/dashboard/#/http/routers

![http](https://user-images.githubusercontent.com/117428887/226187993-5344d135-b75f-4333-8f44-16ce88e3aca5.png)

## 5. http://web.spcn10.local/

![Second](https://user-images.githubusercontent.com/117428887/226188018-4386374e-b0d9-4825-b258-32e6be44b8d1.png)
