![Docker Image Version (tag latest semver)](https://img.shields.io/docker/v/idoko/moira-oas/latest?color=green&label=docker&style=flat-square)

# Documentation
If you're new here, better check out the main Moira Alert [README](https://github.com/moira-alert/moira/blob/master/README.md).

The pre-built docker image is available on docker hub at [idoko/moira-oas](https://hub.docker.com/r/idoko/moira-oas).
## Usage
The OpenAPI description files are kept in a folder that corresponds to the path name. To bundle them into a single `yaml` file,
Install [APIDevTools/swagger-cli](https://github.com/APIDevTools/swagger-cli) globally with:
```bash
$ npm -i g swagger-cli
```
The command to merge them is already defined in the `Makefile`. To use it, run
```bash
$ make spec
```
And it will generate the final documentation file in `build/openapi.yml`

Additionally, you can view the documentation locally using Docker by running docker-compose in the project root directory.
```bash
$ docker-compose up
```
Once the containers are up, visit the URL at http://localhost:9900 to view the SwaggerUI documentation or 
http://localhost:9901 to view the Redoc version.

_**NOTE:** To use the "Try it out" feature on SwaggerUI, ensure that `enable_cors` is set to true in your
API config at `/etc/moira/api.yml`._
## Thanks

![SKB Kontur](https://kontur.ru/theme/ver-1652188951/common/images/logo_english.png)

Moira was originally developed and is supported by [SKB Kontur](https://kontur.ru/eng/about), a B2G company based in Ekaterinburg, Russia. We express gratitude to our company for encouraging us to opensource Moira and for giving back to the community that created [Graphite](https://graphite.readthedocs.io) and many other useful DevOps tools.
