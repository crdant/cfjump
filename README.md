# cfjump
Jumpbox Docker image with all required tools to operate and install Cloud Foundry. It works with different workflows, but focuses primarily on [Enaml](http://enaml.pezapp.io/).

v0.1 includes:

- Ubuntu:xenial official base image
- Several Linux troubleshooting tools, from `dig` to `iPerf`.
- `bosh-init` (0.0.96)
- [BOSH](http://bosh.io/) CLI (latest)
- [uaac](https://docs.cloudfoundry.org/adminguide/uaa-user-management.html) CLI (latest)
- `cf` CLI (latest)
- Golang (1.7.1)
- [Concourse](http://concourse.ci/) `fly` CLI (latest)
- [Vault](https://www.vaultproject.io/) (0.6.1)
- [Terraform](https://www.terraform.io/) (0.7.3)
- `safe` CLI, [an alternative Vault CLI](https://github.com/starkandwayne/safe) (latest)
- [certstrap](https://github.com/square/certstrap) (latest)
- [Spiff](https://github.com/cloudfoundry-incubator/spiff) (latest)
- [Spruce](http://spruce.cf/) (latest)
- [Genesis](https://github.com/starkandwayne/genesis) (latest)

For [Enaml](http://enaml.pezapp.io/), since it's in very active development, the `$HOME/bin/update_enaml.sh` is there to download and register the latest versions. Check the Dockerfile to see the locations and actions.

## Building
You can just get this image from Docker Hub by:

```
docker pull ramxx/cfjump
```

Or if you prefer to build it yourself:

```
git clone https://github.com/RamXX/cfjump
cd cfjump
docker build -t ramxx/cfjump:latest -t ramxx/cfjump:v0.1 .
docker push ramxx/cfjump
```
Increment the version if you change this.

## Running
First, make sure you can run instances as a regular unprivileged user. This container will create an internal user with uid and gid of 1000, same as the default in Ubuntu, which makes easier to share folders.

To run a brand new instance:

```
mkdir shared_vol
docker run --name="Jumpbox1" -it -v $(pwd)/shared_vol:/ops/home \
ramxx/cfjump /bin/bash
```

This container will map its $HOME to the `shared_vol` directory.
Important files, like the BOSH session state and others should be placed here.

If you exit that shell, the container will remain in stopped mode. In order to go back into the same container again, do this:

```
docker start -ai Jumpbox1
```
Which will put you in the same place as you were before. You can use different jumpbox instances for different sessions, users, environments, etc, as long as you use different shared folders.

## Contributing
Please submit pull requests with any correction or improvement you want to do. I hope this is useful to other folks.