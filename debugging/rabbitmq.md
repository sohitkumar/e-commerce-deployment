Issue: Users report delays in order processing, suspecting issues with the RabbitMQ message queue.


When users report order processing delays in a RabbitMQ system, it usually means messages (orders) are getting stuck or processing slowly in the queue.

1. Health Check Commands

    a. kubectl get pods -l app=rabbitmq
    b. kubectl logs rabbitmq-pod-1
    c. kubectl get svc -l app=rabbitmq

2. Check pod resource limit: there may be issue with (Resource limits too low, Wrong storage class, Missing credentials)

    a. kubectl describe pod rabbitmq-pod-1

3. Check connectivity inside pod
    a. kubectl exec -it rabbitmq-pod-1 -- rabbitmqctl ping


4. Last but not least:  We need to actually optimized at code level.
