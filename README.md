# ebd-deploy

## Usage

```
$ ebd-deploy [<environment name>] [<region>]
```

## Getting Started

- create AWS Elastic Beanstalk application and environment
- push your docker image to registry
- `ebd-deploy`

```
$ cat ebd.json
{
  "commands": {
    "yaml2json": "remarshal -if yaml -of json"
  },
  "eb": {
    "config_yaml": ".elasticbeanstalk/config.yml",
    "application": {
      "version_format": "%Y%m%d%H%M%S-{revision}"
    },
    "bundle": {
      "input": "Dockerrun.aws.json .ebextensions",
      "output": "bundle.zip",
      "s3": {
        "bucket": "example-ebd-deploy",
        "directory": "bundles"
      }
    }
  }
}
$ ebd-deploy
```
