# Elixir/Phoenix Dockerfile Examples

The files in this repo are what you need to copy into your Elixir/Phoenix project,
based on whether your Phoenix app has a database or not, in order to get it to run on a service such as 
[Digital Ocean's App Platform](https://www.digitalocean.com/products/app-platform/).  Note that the Dockerfile is identical
in both cases, and simply requires that the appropriate environment variables be set (they are specified
in `.envrc.sample`). One can also access Hex packages from a private org by setting the appropriate environment variables.

You can search for the tag `CHANGE ME!` to find the few items that require edits.

One can use whatever base Docker image one wants for the Elixir build image and the app runner image, but it is recommended that
one picks images which have the versions specified; e.g., 

    APP_BUILDER_ELIXIR_DOCKER_IMAGE = hexpm/elixir:1.11.2-erlang-23.1.1-alpine-3.12.0
    APP_RUNNER_DOCKER_IMAGE = alpine:3.12.0

## Sample Phoenix App without DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_without_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_without_db](sample_phoenix_app_without_db)

## Sample Phoenix App with Postgres DB

See the [full repo here](https://github.com/geometerio/sample_phoenix_app_with_postgres_db).  The parts which have been edited
and are not Phoenix-generated boilerplate have been extracted out into the 
subdirectory [sample_phoenix_app_with_postgres_db](sample_phoenix_app_with_postgres_db)

## Creating the Docker image and running the container locally

Use the scripts in `bin/docker` to create the Docker image locally, and also to run it.  You'll need to specify the 
environment variables in `.envrc.sample` first (copy it to `.envrc` if you're using direnv, or to a `.env` file, or 
however you specify environment variables).  No edits should be required to the Dockerfile.  Change the values in
`config/prod.exs` and `config/runtime.exs` to match your app name, repo name, and endpoint.  If your Phoenix app
has a Postgres database, you'll also need to add and modify the files `lib/your_app_name/release.ex` (which runs
database migrations) and `rel/overlays/bin/start_script`.

***DO NOT*** forget to set `server: true` in `config/prod.exs` for your endpoint!

## Deploying on Digital Ocean's App Platform

Here are the steps required to deploy on Digital Ocean's App platform (after you have copied the required files
into your project):

1) Create an app on DO
1) Set the environment variables shown in `.envrc.sample` in Overview -> Components -> Environment Variables
1) Create a CNAME in your DNS provider for the new Digital Ocean app domain name.
1) Set the environment variable `CANONICAL_HOST` in DO to be this new CNAME value.
1) Under the Digital Ocean Apps -> Settings -> Domains & Certificates, add the new CNAME (this will cause a certificate 
   to get auto-generated).
