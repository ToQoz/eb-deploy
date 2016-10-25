# eb-deploy

## Usage

```
$ eb-deploy [<environment name>] [<region>]
```

## Requirements

- [jq](https://github.com/stedolan/jq)
- [aws-cli](https://github.com/aws/aws-cli)
- yaml -> json command ([remarshal](https://github.com/dbohdan/remarshal), [yaml2json](https://github.com/dbohdan/remarshal), `ruby -e "\"require 'yaml'; require 'json'; STDOUT.puts YAML.load(STDIN.read).to_json\""` etc...)

## Example: deploy docker container

- create AWS Elastic Beanstalk application and environment
- `eb init`
- push your docker image to registry
- `eb-deploy`

```
$ cat eb-deploy.json
{
  "commands": {
    "yaml2json": "ruby -e \"require 'yaml'; require 'json'; STDOUT.puts YAML.load(STDIN.read).to_json\""
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
        "bucket": "example-eb-deploy",
        "directory": "bundles"
      }
    }
  }
}
$ eb-deploy
updating: Dockerrun.aws.json (deflated 35%)
updating: .ebextensions/ (stored 0%)
updating: .ebextensions/xxx.config (deflated 53%)
updating: .ebextensions/yyy.config (deflated 59%)
upload bundle: upload: ./bundle.zip to s3://example-eb-deploy/bundles/20160108190330-2x9f29.zip
create app version: app=zzz version=20160108190330-2x9f29
update environment: app=zzz version=20160108190330-2x9f29 env=zzz-prod
2016-01-08T10:03:34.236Z | INFO | Environment update is starting.
2016-01-08T10:03:39.978Z | INFO | Deploying new version to instance(s).
2016-01-08T10:04:40.794Z | INFO | Environment health has transitioned from Ok to Info. Command is executing on all instances.
2016-01-08T10:04:52.840Z | INFO | New application version was deployed to running EC2 instances.
2016-01-08T10:04:52.884Z | INFO | Environment update completed successfully.
```
