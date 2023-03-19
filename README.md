## spcn10
## Natchapon Supatsorn 
## Kubernetes In Windows

## Wakatime
https://wakatime.com/@spcn17/projects/qknlnznyzo

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
        
        ![image](https://user-images.githubusercontent.com/119097836/226183877-99da52f9-aefa-49da-847d-550a43801da7.png)

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
   ![image](https://user-images.githubusercontent.com/119097836/226183616-bbfe2bad-c10d-4f23-894f-f6ce7c41a27c.png)

2. Run with open dashboard
   ```ruby
   minikube dashboard
   ```
   ![image](https://user-images.githubusercontent.com/119097836/226185662-d2692848-4b5d-4116-9110-47905405da2b.png)

3. Test services
   ```ruby
   minikube service hello-minikube
   ```
   ![image](https://user-images.githubusercontent.com/119097663/224907641-f32599e8-afd0-4a9e-8bf5-f8a59c476752.png)

4. Test Stop minikube
   ```ruby
   minikube pause
   ```
   ![image](https://user-images.githubusercontent.com/119097836/226185662-d2692848-4b5d-4116-9110-47905405da2b.png)

## 4. Install traefik
1. Install Traefik Resource Definitions
   ```ruby
   kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
   ```
    ![image](https://user-images.githubusercontent.com/119097836/226183391-8695b9c9-58fc-4579-a7ae-b0c3461ccafa.png)

2. Install RBAC for Traefik
   ```ruby
   kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.9/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml
   ```
    ![image](https://user-images.githubusercontent.com/119097836/226183302-1ee43bad-92bc-490a-b152-10d4b3641aa7.png) 

3. Install Traefik Helmchart
   ```ruby
   helm repo add traefik https://traefik.github.io/charts 
   helm repo update 
   helm install traefik traefik/traefik 
   ```
    ![image](https://user-images.githubusercontent.com/119097836/226183131-b50d850c-2fad-433d-9195-460e5ca92dd6.png)

4. Verify service is running
   ```ruby
   kubectl get svc -l app.kubernetes.io/name=traefik
   kubectl get po -l app.kubernetes.io/name=traefik
   ```
    ![image](https://user-images.githubusercontent.com/119097663/226110849-021d582a-9f75-4685-94c1-2b1569d90ec5.png)

5. copy user in dashboard-secret place it at user in traefik-dashboard

   ![image](https://user-images.githubusercontent.com/119097836/226182889-ccb98c0b-15ff-4bb5-8c6c-d78d948b580e.png)

   ![image](https://user-images.githubusercontent.com/119097836/226182737-5386691f-26c0-4c30-852c-0fd31860dfb8.png)

   ![image](https://user-images.githubusercontent.com/119097836/226182939-127db28b-5e6a-48f5-a9ea-496880f5fa44.png)

   ![image](https://user-images.githubusercontent.com/119097663/226111244-a7ec1e11-8f01-4070-88ba-0c7a88f83cc1.png)

6. Deploy
   ```ruby
   kubectl apply -f . 
   ```
   ![image](https://user-images.githubusercontent.com/119097663/226111342-4fa25c0d-bdf7-4beb-95fb-dc99e68fc341.png)

## Result

## 1. dashboard

![image](https://user-images.githubusercontent.com/119097836/226182650-3427e2bd-d262-4d67-8c74-7023198916ef.png)

## 2. https://traefik.spcn17.local/dashboard/#/http/routers

![image](https://user-images.githubusercontent.com/119097836/226182456-b259a675-7c5f-4c68-845a-65e11d196f4b.png)

## 5. http://web.spcn17.local/

![image](https://user-images.githubusercontent.com/119097836/226182353-df88ba9b-38a6-4434-afdd-445047ddc80e.png)