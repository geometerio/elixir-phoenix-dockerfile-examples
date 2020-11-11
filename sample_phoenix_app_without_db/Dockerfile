# Adapted from: https://hexdocs.pm/phoenix/releases.html#containers
ARG APP_BUILDER_ELIXIR_DOCKER_IMAGE
ARG APP_RUNNER_DOCKER_IMAGE

FROM $APP_BUILDER_ELIXIR_DOCKER_IMAGE AS app_builder
ENV MIX_ENV=prod
WORKDIR /app

ARG ADDITIONAL_LINUX_PACKAGES=""
RUN apk add --no-cache build-base npm git python3 $ADDITIONAL_LINUX_PACKAGES && \
  mix local.hex --force && \
  mix local.rebar --force

COPY mix.exs mix.lock ./
COPY config config
COPY lib lib
COPY rel rel
# See excluded files in .dockerignore for priv and assets:
COPY priv priv
COPY assets assets

ARG HEX_PRIVATE_ORG=""
ARG HEX_PRIVATE_ORG_READ_ONLY_KEY=""
RUN set -eux; \
  if [ ! -z "${HEX_PRIVATE_ORG}" ] && [ ! -z "${HEX_PRIVATE_ORG_READ_ONLY_KEY}" ]; then \
    mix hex.organization auth ${HEX_PRIVATE_ORG} --key ${HEX_PRIVATE_ORG_READ_ONLY_KEY}; \
  fi

RUN mix do deps.get --only prod, deps.compile && \
  npm --prefix ./assets ci --progress=false --no-audit --loglevel=error && \
  npm run --prefix ./assets deploy && \
  mix phx.digest && \
  mix do compile, release

# -------------------------------------------------------------------------------------------------
ARG APP_RUNNER_DOCKER_IMAGE
FROM $APP_RUNNER_DOCKER_IMAGE AS app_runner
ARG APP_NAME
EXPOSE 4000
WORKDIR /app

RUN apk add --no-cache openssl ncurses-libs && \
  chown nobody:nobody /app

USER nobody:nobody
COPY --from=app_builder --chown=nobody:nobody /app/_build/prod/rel/$APP_NAME ./
ENV HOME=/app

RUN set -eux; \
  if [ ! -e /app/bin/start_script ]; then \
    ln -s /app/bin/$APP_NAME /app/bin/start_script; \
  fi

CMD ["bin/start_script", "start"]
