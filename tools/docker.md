# Docker

## `docker-compose`

### [`expose`][expose] vs [`ports`][ports]

`expose` is used to expose ports without publishing them to the host machine,
meaning they're only accessible by linked services.

Think "private" (`expose`) vs "public" (`ports`)

<!-- prettier-ignore-start -->

[expose]: https://docs.docker.com/compose/compose-file/#expose
[ports]: https://docs.docker.com/compose/compose-file/#ports

<!-- prettier-ignore-end-->
