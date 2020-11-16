# pod-dev-syncer

This script is developed to be a tool while developing on a kubernetes cluster. \
Certain program languages, like PHP, do not require you to always rebuild the container image. \
With this script you can keep an watch job on your development directories and have changes uploaded to your containers automaticly on filesave.
This will enable you to rapidly develop a new application without the need to have to wait for image builds.

The container you are syncing to does need to have write rights in the selected locations, you also need to be logged in into the kubernetes cluster.

## Requirements

This script requires you to have the following programs installed and in your path:
  - kubectl - kubernetes client (https://kubernetes.io/docs/tasks/tools/install-kubectl/)
  - yq - tool to read and manipulate yaml files (https://github.com/mikefarah/yq#install)

## Usage

Recommended way to use is to make a `pod-dev-syncer.yaml` in the root of your project and run
```
/path/to/pod-dev-syncer -C /path/to/project/pod-dev-syncer.yaml
```

You can use the commandline options, but that is fully at own risk.

## Settings file example
```
---
settings:
  waitTime: 5                           # (optional) Wait time between loop runs, defaults to 5
  tmpLoc: /tmp                          # (optional) Directory to use for temp files, defaults to /tmp
project:
  namespace: "projectx"                 # Name of the namespace/project
  pod: 'web\-[0-9a-f]+\-[0-9a-z]+'      # regex for the pod name, this must be in single quotes to be valid yaml
  container: "php-fpm"                  # Name of the container in the pod
local:
  basedir: "/home/user/projectx/app"    # (optional) base dir of the code, defaults to the dir the configfile is in
  dirs: 
    - src/html:/var/www/html            # format = source:target 
    - src/config:/var/www/config        #   source: based from basedir or absolute path on local filesystem 
    - /opt/shared/libs:/var/www/libs    #   target: has to be absolute path in container
```