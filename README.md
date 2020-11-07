# README

The files in this repo are what you need to copy into your Elixir/Phoenix project,
based on whether your Phoenix app has a database or not, in order to get it to run on a service such as 
[Digital Ocean's App Platform](https://www.digitalocean.com/products/app-platform/).  Note that the Dockerfile is identical
in both cases, and simply requires that the appropriate environment variables be set (they are specified
in `.envrc.sample`). One can also access Hex packages from a private org.

# Elixir Phoenix Dockerfile Examples

## Sample Phoenix App with Postgres DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_with_postgres_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_with_postgres_db](sample_phoenix_app_with_postgres_db)

## Sample Phoenix App without DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_without_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_without_db](sample_phoenix_app_without_db)

## Creating the Docker image and running the container locally

Use the scripts in `bin/docker` to create the Docker image locally, and also to run it.

## Deploying on Digital Ocean's App Platform

Here are the steps required to deploy on Digital Ocean's App platform (after you have copied the required files
into your project):

1) Create an app
1) Set the environment variables shown in `.envrc.sample`
1) Create a CNAME in your DNS for the new Digital Ocean app domain name.
1) Set the environment variable `CANONICAL_HOST` to be this new CNAME value.
