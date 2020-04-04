# Kubernetes Service Objects

      1. In kubernetes service is an object that is logically mapped to pods based on labels.
       
      2. Service can be single point of contact like loadbalancer for set of pods.
       
      3.  By default Pod is not expose to the internet or outside of the cluster, by using service Pod is exposed to internet.
      
      4.  Any Pod create in future is added to service dynamicaly if pod label is matching with Service lable selector.
        service
