# RC Tech Club Basic Development Environment

This repository is useful to you if you mean to develop an application for the RC Tech Club but you do not need to modify the server itself.

The docker containers that will be deployed are:

1. Prisma
2. Postgres database
3. Phoenix
4. Ladybird

#### To run the deployment

###### Dependencies

- Docker and Docker Compose
- Prisma CLI
- Phoenix

Setup your environment variables. At this repo's root directory:

```
cp .env.sample .env
```

Configure your developer configurations in the newly copied `.env` file to use services like AWS. Note that you have to obtain your unique IAM access key pairs, contact one of the club admins for this.

```
// .env
NODEMAILER_PASSWORD= // you can leave this blank, it is not critical
AWS_ACCESS_KEY_ID= // your iam access key id
AWS_SECRET_ACCESS_KEY= // your iam secret access key
```

Then run the command.

```
docker-compose up -d
```

As a result, you should have:

Phoenix available at: `http://localhost:4000`  
Prisma available at: `http://localhost:4466`  
Ladybird available at: `http://localhost:3001`

However, your prisma server will not have a deployment yet, even though the schema is generated. To run the deployment, you must have cloned `phoenix`.

At `phoenix` root directory:

```
cd prisma
prisma deploy
```

Now, you are ready to start developing.

After you finish, remember to run:

```
docker-compose down
```

This will remove all the components of the deployment.
