# Relplica set

Replica Set ensures how many replica of pod should be running.

        apiVersion: extensions/v1beta1 --------------------->1
        kind: ReplicaSet --------------------------> 2
        metadata:
           name: Tomcat-ReplicaSet
        spec:
           replicas: 3
           selector:
              matchLables:
                 tier: Backend ------------------> 3
              matchExpression:
        { key: tier, operation: In, values: [Backend]} --------------> 4
