
Setup Trino and Superset on Minikube
=====================================



What is Trino?
--------------

Trino is a distributed query engine that processes data in parallel across multiple servers. There are two types of Trino servers, coordinators and workers. The following sections describe these servers and other components of Trino’s architecture.

What is Apache Superset?
-------------------------

Apache Superset is a modern, enterprise-ready business intelligence web application. It is fast, lightweight, intuitive, and loaded with options that make it easy for users of all skill sets to explore and visualize their data, from simple pie charts to highly detailed deck.gl geospatial charts.

Project Aim
------------

For this project I have used the following database to be the backends of trino .
1. Mysql - Inventory
2. Mysql - Petclinic
3. Mongodb Atlas

   I have setup trino and Superset to fetch data from the above DB's .

<img src="/images/trino-superset.jpg" width="60%" height="60%" >

Setup Multiple Databases :
--------------------------

[How to Setup Sample Databases for This Setup?](SetupDatabases.md)



Setup Trino on Minikube :
-------------------------

##### 1. Add Helm repo 

    $ helm repo add trino https://trinodb.github.io/charts

##### 2. Install trino using helm charts 

    $ helm install example-trino-cluster trino/trino
    

##### 3. Verify Pods


##### 4. Update the Config Map [example-trino-cluster-catalog] for trino :

    
   https://github.com/Lord-of-the-Pods/trino-minikube/blob/df8d28a6822921d0f87712a5eb75573b354c2171/trino/trino-catalog.text#L1-L21

##### 5. Restart the Deployments for trino

    $ kubectl rollout restart -n trino-superset deployment example-trino-cluster-coordinator
    

##### 6. Port forward for Trino cli to be able to listen to the trino cluster .

    $ PODNAME=$(kubectl -n trino-superset get pods -o NAME| grep coordinator)
    
    $ kubectl -n trino-superset port-forward $POD_NAME 8080:8080
    
##### 7. Download the trino-cli from the following locaton .

##### 8. Rename and run the .jar file

##### 9. The java application will give you access to the trino cli , fire the queries using the context and calalog name .

 <img src="/images/trino-queries.png" width="80%" height="80%">

Setup Superset :
----------------

##### 1. Add the Superset helm repository

   ```
   $ helm repo add superset https://apache.github.io/superset
    
   "superset" has been added to your repositories
   ```

##### 2. Install Superset using heml and custom values.yaml with sqlalchemy library

   Use the following values.yaml file for superset to configure trino sqlalchemy library

   https://github.com/Lord-of-the-Pods/trino-minikube/blob/3e47a6477740183b88864b68ec2fe15f51a072d8/Superset/values.yaml#L44-L50

   ```
   $ helm upgrade --install --values values.yaml superset superset/superset
   ```


##### 3. Port forward for Trino cli to be able to listen to the trino cluster .

    $ kubectl port-forward service/superset 8088:8088 --namespace superset
    

##### 4. Login into Superset with following credentials : admin / admin

##### 5. Create a Trino Connection in Superset

Goto **Settings > Database Connections** and click on +Database Icon to create a new Database Connection
    
<img src="/images/trino-create-conn.png" width="30%" height="30%">

 use the following connectiong string [modify it accordingly]
    
    trino://trino@example-trino-cluster.trino-superset:8080
    

##### 5. Fire queries to your databases from Superset UI .

 Goto Sql Lab from top menu and fire your SQL queries .

 <img src="/images/superset-result.png" width="80%" height="80%">

   ```
   SELECT * from mysqlinventory.inventory.customers;
   
   SELECT * from mysqlpetclinic.petclinic.owners;
   
   SELECT * from mongo.sample_mflix.users where name like "%Stark%";
   ```

