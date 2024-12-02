

# **Cosmocloud Deployment using Helm**

This README outlines the steps to deploy the Cosmocloud application stack (frontend, backend, and Redis) on Kubernetes using Helm. 

## **Steps to Deploy the Application Using Helm**

### **1. Prerequisites**
- **Kubernetes Cluster**: Ensure you have a running Kubernetes cluster 
- **Helm Installed**: Helm must be installed on your local machine.
- **kubectl Installed**: Ensure `kubectl` is installed and configured to interact with your Kubernetes cluster.

### **2. Clone the Repository**
First, clone the repository that contains the Helm chart for Cosmocloud:
```bash
git clone https://github.com/yourusername/cosmocloud-deploy.git
cd cosmocloud-deploy
```

### **3. Install Helm Chart**
Run the following Helm command to deploy the Cosmocloud application stack:
```bash
helm install testapp cosmocloud-deploy
```

This command will:
- Install the application using the Helm chart.
- Deploy the frontend, backend and Redis services in the Kubernetes cluster.


### **4. Verify Deployment**
Check the status of the deployed pods:
```bash
kubectl get all

```
![Screenshot (326)](https://github.com/user-attachments/assets/d0916148-da4b-45ef-88eb-a9e07745b4af)



### **5. Port Forwarding **
If you want to access the frontend or backend locally via `kubectl` port-forwarding, use the following commands:

1. **Frontend**:
   ```bash
   kubectl port-forward svc/frontend-svc 31000:5173 --address 0.0.0.0
   ```
   ![Screenshot (324)](https://github.com/user-attachments/assets/e37d634e-2c36-4a7a-ab01-aac86ac3d41a)

2. **Backend**:
   ```bash
   kubectl port-forward svc/backend-svc 8000 --address 0.0.0.0
   ```
  


   

### **6. Access the Application**
Once port-forwarding is set up, you can access the frontend and backend services via your browser or `curl`:
- Frontend: `http://<EC2-Public-IP>:31000`
- ![Screenshot (325)](https://github.com/user-attachments/assets/7c383591-0764-486a-be70-4aa8d25f5eb2)

- Backend: `http://<EC2-Public-IP>:30000`
- ![image](https://github.com/user-attachments/assets/a1432472-141c-4488-bd2e-9e705d1f62e6)



---

## **Error Debugged and Found**

While working on the assignment, I ran into an issue with the frontend service. the instructions mentioned that the frontend application should be exposed on port 5175, but when I tried accessing it, it didn’t work. So, I decided to dig deeper.

First, I checked the logs of the frontend pod  and I noticed it was actually listening on port 5173. This seemed odd, so then i went to dockerhub and i searched for shreybatra/sample-frontend image then I looked into the Dockerfile of the provided Docker image (shreybatra/sample-frontend). That’s when I realized the application was actually configured to expose port 5173, not 5175 as mentioned in the assignment.

To fix this, I updated the Helm chart to use port 5173 instead of 5175, and everything worked perfectly after that.

---

## **Cleanup**
To delete the application and all its resources from the Kubernetes cluster:
```bash
helm uninstall testapp --namespace default
```

This will remove all the Kubernetes resources associated with the Helm release.

