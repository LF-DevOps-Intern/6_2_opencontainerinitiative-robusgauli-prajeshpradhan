# Question 2 & 3

> 2. Deploy Postgres Client in cluster(psql)
> 3. Connect Postgres database from Postgres Client using core-dns's 
host name.

These two questions are done using one single command using kubectl, which prompts the psql terminal.

The command simply runs a pod with postgres Client and using the core dns inside the cluster connects to the database:

```
~ kubectl run postgres-client --rm --tty -i --restart='Never' --image postgres:11 --env="PGPASSWORD=postgres" --command -- psql -h postgres -U postgres
```

![Postgres client](screenshots/Screenshot%202021-11-30%20024231.png)


