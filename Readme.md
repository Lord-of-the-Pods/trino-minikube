**What is Trino?**

Trino is a distributed query engine that processes data in parallel across multiple servers. There are two types of Trino servers, coordinators and workers. The following sections describe these servers and other components of Trino’s architecture.



Minikube start . 

**Setup Multiple Databases :**

1. Mysql - Inventory
    Use the following Yaml to Create a Sample database for inventory :

https://github.com/Lord-of-the-Pods/trino-minikube/blob/6eadc2a72de15acde01dfde40e91abbf8d97a19c/databases/mysql-inventory.yaml#L1-L41

    Exec into the pod to test the Database , use the following credentials : petclinic/petclinic

<img src="/images/mysql-inventory.png">
   

4. Mysql2 - Petclinic

     Use the following Yaml to Create a Sample database for Petclinic :

https://github.com/Lord-of-the-Pods/trino-minikube/blob/6eadc2a72de15acde01dfde40e91abbf8d97a19c/databases/mysql-petclinic.yaml#L1-L202

   Exec into the pod to test the Database , use the following credentials : mysqluser/mysqlpw

<img src="/images/mysql-petclinic.png">

4. Mongo

   Coming Soon .!!

6. Postgres

   Coming Soon .!!



** Setup Trino **

1. Add Helm repo 

helm repo add trino https://trinodb.github.io/charts

2. Install trino using helm charts 

helm install example-trino-cluster trino/trino

3. Verify Pods 


4. Access trino from cli

PODNAME=$(kubectl -n trino-superset get pods -o NAME| grep coordinator)

kubectl -n trino-superset port-forward $POD_NAME 8080:8080








**Update the Config Map for trino : **

example-trino-cluster-catalog


5. Restart the Deployments for trino

'kubectl rollout restart -n trino-superset deployment example-trino-cluster-coordinator'


6. Port forward for Trino cli to be able to listen to the trino lcuster .





**setup Superset** 





trino://trino@example-trino-cluster.trino-superset:8080
