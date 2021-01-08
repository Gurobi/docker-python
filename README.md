![Logo](https://www.gurobi.com/wp-content/uploads/2018/12/logo-final.png "Gurobi Optimization")
# Quick reference
Maintained by: [Gurobi Optimization](https://www.gurobi.com)

Where to get help: [Gurobi Support](https://www.gurobi.com/support/), [Gurobi Documentation](https://www.gurobi.com/documentation/)

# Supported tags and respective Dockerfile links
## Simple Tags
* [9.1.1, latest](https://github.com/Gurobi/docker-python/blob/master/9.1.1/Dockerfile)

# Quick reference (cont.)

Supported architectures: linux amd64

Published image artifact details: https://github.com/Gurobi/docker-python

Gurobi images:
- [gurobi/optimizer](https://hub.docker.com/repository/docker/gurobi/optimizer): Gurobi Optimizer (full distribution)
- [gurobi/python](https://hub.docker.com/repository/docker/gurobi/python): Gurobi Optimizer (Python API only)
- [gurobi/modeling-examples](https://hub.docker.com/repository/docker/gurobi/modeling-examples): Optimization modeling examples (distributed as Jupyter Notebook)
- [gurobi/compute](https://hub.docker.com/repository/docker/gurobi/compute): Gurobi Compute Server
- [gurobi/manager](https://hub.docker.com/repository/docker/gurobi/manager): Gurobi Cluster Manager

# What is `gurobi/python`?
The Gurobi Optimizer is the fastest and most powerful mathematical programming solver available 
for your LP, QP and MIP (MILP, MIQP, and MIQCP) problems. 
More info at the [Gurobi Website](https://www.gurobi.com/products/gurobi-optimizer/).

The Gurobi Optimizer comes with a Python extension module called “gurobipy” that offers convenient 
object-oriented modeling constructs and an API to all Gurobi features. 
More info in the [Quick Start Guide](https://www.gurobi.com/documentation/current/quickstart_windows/cs_python.html).

The `gurobi/python` image provides a base Docker image for applications that use the Gurobi Python 
interface.

## Getting a Gurobi license

This image comes with a `Limited License` that allows you to solve small optimization problems.
To solve larger problems, you will need to get a license that works with your application
running in Docker containers.  You have a few options:

* A [Gurobi Compute Server](https://www.gurobi.com/products/gurobi-compute-server/) 
allows you to seamlessly offload your optimization computations onto one or more dedicated 
optimization servers grouped in a cluster. Users and applications can share the servers 
thanks to advanced queuing and load balancing capabilities. Users can monitor jobs, and 
administrators can manage the servers. The cluster manager provides additional features
and security management. The Compute Server and the Cluster Manager must 
be installed outside of the Docker cluster.

* The [Gurobi Cloud](https://www.gurobi.com/products/gurobi-instant-cloud/) is a simple and 
cost-effective way to get up and running with powerful Gurobi optimization software running 
on cloud services. It allows you to launch one or more computers, pre-loaded with Gurobi 
software and dedicated to you on Microsoft Azure® and Amazon Web Services®. 

* A [Gurobi Token Server](https://www.gurobi.com/documentation/current/quickstart_linux/creating_a_token_server_cl.html) 
runs on a dedicated machine outside of the Docker cluster. Client programs running in Docker containers can
request tokens from the token server.

* Please contact your sales representative at [sales@gurobi.com](mailto:sales@gurobi.com) to discuss additional options. 

Note that other standard license types (NODE, Academic) do not work with Docker.

## Using the client license

If you aren't using the embedded `Limited License`, you will need to specify a set of properties to 
connect to a Compute Server cluster, the Gurobi Cloud, or a token server. To do so,
you have the following options:
* Mounting the client license file:
You can store connection parameters in a client license file (typically called `gurobi.lic`) 
and mount it to the container. This option provides a simple approach for testing.

* Setting parameters with the API:
When your application creates a [Gurobi environment](https://www.gurobi.com/documentation/current/refman/py_env.html),
you can specify connection parameters through the API. The parameter values can come from 
environment variables, a database or from other sources as required by your application. 
This is the recommended approach for production applications.

A quick guide to the appropriate API parameters and license file properties is available [here](https://github.com/Gurobi/docker-optimizer/blob/master/PARAMS.md).

## Testing with a simple examples
In the following command-line examples, we will run an example stored on 
your local machine. The example must be stored in a local directory that
will be mounted to the instance.

In the current directory (retrieved using `$PWD`), create a sub-directory that
contains the example you would like to run:
 
    Directory: `$PWD/models`
    Content: `poolsearch.py`
    
See some [model examples](https://github.com/Gurobi/docker-python/tree/master/models).
    
# How to use this image?

## Optimizing with `gurobi/python` instance

The following command line starts the `gurobi/python` container, mounts a directory 
that contains a few examples, and then runs the example `poolsearch.py` using 
the embedded limited license:

```console
$ docker run --volume=$PWD/models:/models \
             gurobi/python  /models/poolsearch.py
```

... where `$PWD` is the current directory.

If you need to use a Compute Server, Gurobi Cloud or a token server, you can mount 
the client license file in readonly mode from the local location `$PWD`:

```console
$ docker run --env=GRB_CLIENT_LOG=3  \
             --volume=$PWD/gurobi.lic:/opt/gurobi/gurobi.lic:ro \
             --volume=$PWD/models:/models:ro \
             gurobi/python  /models/poolsearch.py
```


Note that this command also enables client logging by using
the `GRB_CLIENT_LOG` variable.
 
## ... via docker stack deploy or docker-compose
Example `docker-compose.yml` for a Gurobi python instance:

```
version: '3.7'
services:
  gurobi:
    image: gurobi/python:latest
    volumes:
      - ./models:/models:ro
      - ./gurobi.lic:/opt/gurobi/gurobi.lic:ro

```

Run `$ docker-compose run gurobi /models/poolsearch.py `

## How to use this image as a base image?
This image can also be used as a base image for applications using Gurobi from Python. 


File `Dockerfile`
```
FROM gurobi/python:latest
...

RUN ...
...

```

## Environment Variables

The following environment variables can be used:

* `GRB_CLIENT_LOG`: Turns Client Server logging on or off. Options are off (0), only error messages (1), information and error messages (2), or (3) verbose, information, and error messages. 

# License

View [End User License Agreement](https://www.gurobi.com/wp-content/uploads/2020/11/EULA_standard.pdf) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other 
licenses (such as Bash, etc from the base distribution, along with any direct or indirect 
dependencies of the primary software being contained).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use 
of this image complies with any relevant licenses for all software contained within.
