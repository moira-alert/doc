# Documentation

[![Documentation Status](https://readthedocs.org/projects/moira/badge/?version=latest)](http://moira.readthedocs.org/en/latest/?badge=latest)

If you're new here, better check out our main [README](https://github.com/moira-alert/moira/blob/master/README.md).

Compiled version of this documentation is available on [Read the Docs](https://moira.readthedocs.io) site.
## Contributing
The OpenAPI configuration files are kept in the `api/` folder. To bundle them into a single `yaml` file,
Install [APIDevTools/swagger-cli](https://github.com/APIDevTools/swagger-cli) globally with:
```bash
$ npm -i g swagger-cli
```
Next, change into the `api` folder with `cd api` and bundle all of the OpenAPI files into a single `swagger.yml` file.
```bash
$ swagger-cli bundle main.yml -o swagger.yml
```

Additionally, you can view the documentation locally using Docker by running docker-compose in the project root directory.
```bash
$ docker-compose up
```
Once the containers are up, visit the URL at http://localhost:9900 to view the Redoc-rendered documentation or 
http://localhost:9901 to view the SwaggerUI version.

_**NOTE:** To use the "Try it out" feature on SwaggerUI, ensure that `enable_cors` is set to true in your
API config at `/etc/moira/api.yml`._
## Thanks

![SKB Kontur](https://kontur.ru/theme/ver-1652188951/common/images/logo_english.png)

Moira was originally developed and is supported by [SKB Kontur](https://kontur.ru/eng/about), a B2G company based in Ekaterinburg, Russia. We express gratitude to our company for encouraging us to opensource Moira and for giving back to the community that created [Graphite](https://graphite.readthedocs.io) and many other useful DevOps tools.
