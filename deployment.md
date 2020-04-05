# configuration file of Deployment looks like this.

        apiVersion: extensions/v1beta1 --------------------->1
        kind: Deployment --------------------------> 2
        metadata:
           name: Tomcat-ReplicaSet
        spec:
           replicas: 3
           template:
              metadata:
                 lables:
                    app: Tomcat-ReplicaSet
                    tier: Backend
           spec:
              containers:
                 - name: Tomcatimage:
                    tomcat: 8.0
                    ports:
                       - containerPort: 7474
                       

# Deployment Strategies
Deployment strategies help in defining how the new RC should replace the existing RC.


 Recreate − This feature will kill all the existing RC and then bring up the new ones. This results in quick deployment however it will result in downtime when the old pods are down and the new pods have not come up.
 

 Rolling Update − This feature gradually brings down the old RC and brings up the new one. This results in slow deployment, however there is no deployment. At all times, few old pods and few new pods are available in this process.
 
