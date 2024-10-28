Frontend Accessibility:

Issue: The frontend service is not accessible externally post-deployment.

1. Verify Frontend Service Configuration

First, ensure the Service type is NodePort or LoadBalancer, if dealing with cloud providers then verify ClusterIP with Ingress if you’re managing external access via an Ingress controller.

We can go and check : deployment/frontend/fe-service.yaml

2. Verify Ingress Configuration

Ensure the Ingress resource is correctly set up to route traffic to the frontend-service.

We can go and check : deployment/ingress/ingress.yaml

3. We can also verify network policies if using any custom conf.

4. We should also verify Ingress Controller
   
   kubectl get pods -n ingress-nginx

5. Also, we can check logs : kubectl logs i ingress -n ingress-nginx

6. DNS & External IP Verification
    a. Ensure your DNS points to the external IP address assigned to the Ingress.
    b. Use kubectl get svc to check if an external IP has been assigned, and update your DNS if needed.

7. Check Pod Health and Logs
    a. kubectl get pods -l app=frontend
    b. kubectl logs fe-pod-1