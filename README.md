# Elixir/Phoenix Dockerfile Examples

The files in this repo are what you need to copy into your Elixir/Phoenix project,
based on whether your Phoenix app has a database or not, in order to get it to run on a service such as 
[Digital Ocean's App Platform](https://www.digitalocean.com/products/app-platform/).  Note that the Dockerfile is identical
in both cases, and simply requires that the appropriate environment variables be set (they are specified
in `.envrc.sample`). One can also access Hex packages from a private org by setting the appropriate environment variables.

You can search for the tag `CHANGE ME!` to find the few items that require edits (and there are only items for the case with a 
database).

One can use whatever base Docker image one wants for the Elixir build image and the app runner image, but it is recommended that
one picks images which have the versions specified; e.g., 

    APP_BUILDER_ELIXIR_DOCKER_IMAGE = [hexpm/elixir:1.11.2-erlang-23.1.1-alpine-3.12.0](https://hub.docker.com/layers/hexpm/elixir/1.11.2-erlang-23.1.1-alpine-3.12.0/images/sha256-d38e5ac0a87ab8b400713baef08cd4f489ea718bf22ccae3627eb43b95d541be?context=explore)
    APP_RUNNER_DOCKER_IMAGE = [alpine:3.12.0](https://hub.docker.com/layers/alpine/library/alpine/3.12.0/images/sha256-3b3f647d2d99cac772ed64c4791e5d9b750dd5fe0b25db653ec4976f7b72837c?context=explore)

## Sample Phoenix App without DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_without_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_without_db](sample_phoenix_app_without_db)

## Sample Phoenix App with Postgres DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_with_postgres_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_with_postgres_db](sample_phoenix_app_with_postgres_db)

## Creating the Docker image and running the container locally

Use the scripts in `bin/docker` to create the Docker image locally, and also to run it.

## Deploying on Digital Ocean's App Platform

Here are the steps required to deploy on Digital Ocean's App platform (after you have copied the required files
into your project):

1) Create an app
1) Set the environment variables shown in `.envrc.sample`
1) Create a CNAME in your DNS for the new Digital Ocean app domain name.
1) Set the environment variable `CANONICAL_HOST` to be this new CNAME value.
1) Under the Digital Ocean Apps -> Settings -> Domains & Certificates, add the new CNAME (this will
   cause a certificate to get auto-generated).
