# Question 1

>  1. Deploy Postgres database using PVC & PV cluster

To Deploy postgres database using PVC and PV cluster we just need to make a yaml manifest file. 
Official postgresSQL image can be taken from dockerhub. Persistent Volume and Persistent 
Volume Claim is used to store and persist the data even if the pods are deleted. So the yaml 
configuration for postgres is

```
apiVersion: apps/v1
kind: Deployment
metadata:
 name: postgres
spec:
 replicas: 1
 selector:
 matchLabels:
 app: postgres
 template:
 metadata:
 labels:
 app: postgres
 spec:
 volumes:
 - name: postgres-pv-storage
 persistentVolumeClaim:
 claimName: postgres-pv-claim
 containers:
 - name: postgres
 image: postgres:11
 imagePullPolicy: IfNotPresent
 ports:
 - containerPort: 5432
 env:
 - name: POSTGRES_PASSWORD
 valueFrom:
 secretKeyRef:
 name: postgres-secret-config
 key: password
 - name: PGDATA
 value: /var/lib/postgresql/data/pgdata
 volumeMounts:
 - mountPath: /var/lib/postgresql/data
 name: postgres-pv-storage
---
apiVersion: v1
kind: Secret
metadata:
 name: postgres-secret-config
type: Opaque
data:
 password: cG9zdGdyZXMK
---
apiVersion: v1
kind: PersistentVolume
metadata:
 name: postgres-pv-volume
 labels:
 type: local
spec:
 storageClassName: manual
 capacity:
 storage: 5Gi
 accessModes:
 - ReadWriteOnce
 hostPath:
 path: "/mnt/data"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: postgres-pv-claim
spec:
 storageClassName: manual
 accessModes:
 - ReadWriteOnce
 resources:
 requests:
 storage: 1Gi
```

Here first the postgresSQL pod is created using the **Deployment** component in the cluster, the root password is set using the **secrets** component, and finally the **Persistent Volume** and **Persistent Volume Claims** are created and configured with the deployment created.

To apply these rules, we will use **kubectl** command as:

```
~ kubectl apply -f postgres.yml
```

![Apply](screenshots/Screenshot%202021-11-30%20022503.png)

Again, expose the deployment as **Nodeport** to use the application

```
~ kubectl expose deployment postgres --type=NodePort --port=5432
```

![Expose for Service](screenshots/Screenshot%202021-11-30%20023421.png)