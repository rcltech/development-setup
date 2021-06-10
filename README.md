# RC Tech Club Basic Development Environment

This repository is meant for RC Tech Club application developers who wish to use Phoenix without on-the-fly Phoenix code development.

The docker containers that will be deployed are

1. Postgres database
2. Phoenix
3. Ladybird

## Quick Start

###### Dependencies

- Docker and Docker Compose
- Phoenix

Setup your environment variables. At this repo's root directory, run

```sh
cp .env.sample .env
```

Configure your developer configurations in the newly copied `.env` file to use services like AWS. Note that you have to obtain your unique IAM access key pairs, contact one of the club admins for this.

```
// .env
NODEMAILER_PASSWORD= // you can leave this blank, it is not critical
AWS_ACCESS_KEY_ID= // your iam access key id
AWS_SECRET_ACCESS_KEY= // your iam secret access key
```

Then run

```sh
docker-compose up -d
```

As a result, you should have:

Phoenix available at: `http://localhost:4000`  
Ladybird available at: `http://localhost:3001`

Now, you are ready to start developing!

After you finish, remember to run

```sh
docker-compose down
```

This removes all the containers.

## Advanced

### Pulling the latest images
It is recommended to pull the latest images before spinning up the containers, especially if `phoenix` or `ladybird` images have been updated, and you want to use them.

```sh
docker-compose pull
```

## Troubleshooting

Your containers can fail in several ways. Some common issues have been identified, and possible fixes are written here.

### Postgres data directory

```
The data directory was initialized by PostgreSQL version 12, which is not compatible with this version 13.2
```

This means the volume mounted by docker is based-off of an older version of postgres, while the postgres image being used is not compatible with that. Simply remove this docker volume.

```sh
docker volume rm development-setup_rctech-dev
```

*A docker volume basically allows long term storage of postgres data, even after the postgres container is brought down, and removed.*

## Additional Information

### Prisma migration and client generation

Phoenix's `rctechclub/phoenix:dev` image makes sure to run `prisma migrate deploy`, and `prisma generate` to make sure that
- the `postgres` database is in-sync with the prisma schema
- the `@prisma/client` is connected to the correct database on your local machine

**The production images (`rctechclub/phoenix:latest` or `rctechclub/phoenix:prod`) do not perform this step.**
