# redis-tools

<p>
  <a href="https://hub.docker.com/repository/docker/jc21/redis-tools">
    <img src="https://img.shields.io/docker/stars/jc21/redis-tools.svg?style=for-the-badge">
  </a>
  <a href="https://hub.docker.com/repository/docker/jc21/redis-tools">
    <img src="https://img.shields.io/docker/pulls/jc21/redis-tools.svg?style=for-the-badge">
  </a>
</p>

This is a docker image compiled with some redis commands:
- redis-cli
- [redis-renamer](https://github.com/jc21/redis-renamer)
- [redis-migrator](https://github.com/jc21/redis-migrator)

The following architectures are supported for all images:

- amd64
- arm64

### Usage:

```
docker run jc21/redis-tools redis-cli -h
docker run jc21/redis-tools redis-renamer -h
docker run jc21/redis-tools redis-migrator -h
```
