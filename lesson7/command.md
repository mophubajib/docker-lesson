1.
```bash
docker run `
>> -e POSTGRES_PASSWORD=postgres `
>> -e POSTGRES_USER=postgres `
>> -d `
>> --name postgres `
>> --network postgres-my-db `
>> -p 5432:5432 `
>> postgres
```

```bash
    psql -U postgres
    create database my_db OWNER postgres;
    \l
```

2. docker build -f Dockerfile --no-cache-filter=cloner -t simple-crud-external-yaml:0.0.1 .
3. docker run -p 8084:8080 --network postgres-my-db simple-crud-external-yaml:0.0.1