STEPS:
 1 - understanding the YAML: It has 3 resources 
 2 - First Service : redis-service
     labels dont match 
     changed app: database to app:redis under selector
 3 - Second Deployment: redis-db
     no Change
 4 - Third Deployment: web-app
     no server running on 8080
     redis install command missing
     Changes : added pip  install redis and changed command ["python"] to command ["sh"]
     readinessProbe:
          httpGet:
            path: /
            port: 8080 
     TO 
       readinessProbe:
          exec:
            command:
              - python
              - -c
              - "import redis, os; redis.Redis(host=os.getenv('REDIS_HOST','redis-service'), port=6379).ping()"

After debugging 
  kubectl apply -f manifests.yaml - to apply service and  deployments 
  kubectl get pods - to check if the pods are up
  kubectl get deployments -to  check deployments
  kubectl log deployments/web-app -to check logs of deployments
 
