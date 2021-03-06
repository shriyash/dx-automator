# The Developer Experience (DX) Automator

This tool is intended to help make managing multiple Github repositories much easier for DX, DevRel, and Open Source Engineering teams.

Please check out the [development branch](https://github.com/sendgrid/dx-automator/tree/development) to see what's going on.

We will update this README and the master branch, as well as deploy the code to pypi once the MVP is ready!

## Contributing
Everyone who participates in our repo is expected to comply with our [Code of Conduct](https://github.com/sendgrid/dx-automator/blob/development/CODE_OF_CONDUCT.md).

We welcome [contributions](https://github.com/sendgrid/dx-automator/blob/development/CONTRIBUTING.md) in the form of issues, pull requests and code reviews. Or you can simply shoot us an [email](mailto:dx@sendgrid.com).

## Attributions
We believe in open source and want to give credit where it's due. We used the amazing tutorial at [testdriven.io](https://testdriven.io) to guide us in setting a solid foundation using flask, docker, and (eventually) node and react. From this tutorial, we began to build and iterate.

## Usage - Local

### Create Local Docker Machine

```bash
docker-machine create -d virtualbox dx-automator-dev
```

### Deploy Locally

Setup your environment variables:

```bash
cp ./services/github/.env_sample ./services/github/.env
cp ./services/looker/.env_sample ./services/github/.env
```


Install:

```bash
./source_the_things.sh
docker-machine start dx-automator-dev
docker-machine env dx-automator-dev
eval $(docker-machine env dx-automator-dev)
docker-compose -f docker-compose-dev.yml up -d --build
DX_IP="$(docker-machine ip dx-automator-dev)"
./scripts/setup-local-db
```

Run these commands to test if everything is working correctly.

```
curl http://$DX_IP/tasks/ping
curl http://$DX_IP/tasks
curl http://$DX_IP/users/ping
curl http://$DX_IP/users
curl http://$DX_IP/github/ping
curl http://$DX_IP/github/members
curl http://$DX_IP/github/is_member/<github_username>
```

Grab the IP address.

```
echo $DX_IP
```

And now paste that IP into your browser and you should see a task list.

### Connect to Local DB

```bash
docker-compose -f docker-compose-dev.yml exec users-db psql -U postgres
# \c users_dev
# select * from users;
# \q
```

## Usage - Cloud

### Create AWS Docker Machine

```bash
docker-machine create --driver amazonec2 dx-automator-prod
```

### Deploy to AWS

```bash
docker-machine env dx-automator-prod
eval $(docker-machine env dx-automator-prod)
docker-compose -f docker-compose-prod.yml up -d --build
DX_IP="$(docker-machine ip dx-automator-prod)"
curl http://$DX_IP/users/ping
curl http://$DX_IP/users
```
