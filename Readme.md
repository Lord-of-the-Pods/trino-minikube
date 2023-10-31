**What is Trino?**

Trino is a distributed query engine that processes data in parallel across multiple servers. There are two types of Trino servers, coordinators and workers. The following sections describe these servers and other components of Trino’s architecture.


**Setup Multiple Databases :**

[How to Setup Sample Databases for This Setup?](SetupDatabases.md)



**Setup Trino**

1. Add Helm repo 

    ```
    $ helm repo add trino https://trinodb.github.io/charts
    ```

2. Install trino using helm charts 

    ```
    $ helm install example-trino-cluster trino/trino
    ```

3. **Verify Pods**


4. **Update the Config Map for trino :**

    example-trino-cluster-catalog
    
    https://github.com/Lord-of-the-Pods/trino-minikube/blob/3e47a6477740183b88864b68ec2fe15f51a072d8/trino/trino-catalog.text#L1-L21

5. **Restart the Deployments for trino**

    ```
    $ kubectl rollout restart -n trino-superset deployment example-trino-cluster-coordinator
    ```

6. **Port forward for Trino cli to be able to listen to the trino cluster .**

    ```
    $ PODNAME=$(kubectl -n trino-superset get pods -o NAME| grep coordinator)
    
    $ kubectl -n trino-superset port-forward $POD_NAME 8080:8080
    ```

**setup Superset** 

1. **Add the Superset helm repository**

   ```
   $ helm repo add superset https://apache.github.io/superset
    "superset" has been added to your repositories
   ```

2. Install and run

   ```
   $ helm upgrade --install --values values.yaml superset superset/superset
   ```

   use the following values.yaml file for superset to configure trino alchemy library

   https://github.com/Lord-of-the-Pods/trino-minikube/blob/3e47a6477740183b88864b68ec2fe15f51a072d8/Superset/values.yaml#L44-L50

3. **Port forward for Trino cli to be able to listen to the trino cluster .**

    ```
    $ kubectl port-forward service/superset 8088:8088 --namespace superset
    ```





trino://trino@example-trino-cluster.trino-superset:8080
