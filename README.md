# ci-skeleton
Groovy based Jenkins CI skeleton

### File structure



    |----jobs                    # DSL script files
    |----lib                     # defaults, NOT used now


###    Trying to use:
    ├── resources               # resources for DSL scripts
    ├── src
    │   ├── main
    │   │   ├── groovy          # support classes
    │   │   └── resources
    │   │       └── idea.gdsl   # IDE support for IDEA
    │   └── test
    │       └── groovy          # specs
    └── build.gradle            # build file
    
## Links
    
   * [Jenkins DSL](https://github.com/jenkinsci/job-dsl-plugin)
   * [Jenkins DSL API](https://jenkinsci.github.io/job-dsl-plugin/)
   * [Pipeline plugin](https://jenkins.io/doc/book/pipeline/)
    

# Framework structure

| Folder | Content |
| ------ | ------ |
| /jobs | groovy files with service jobs description |
| /resources | json config files, by services |
| /templates | json config and groovy job template files |
| /src/main/groovy/qa | config classes |
| /templates | templates for config and jobs |
| /shell | put shell scripts here |
| Jenkinsfile.groovy | CI pipeline for backend services |
| update_env.sh | script for updating pod in kubernetis |

## App Services under test

* list
* of
* services

 
## CI Pipeline name convention

For CI Pipeline use next naming:
```
# for job name
job("${config.baseName}/${config.baseName}-ci-${service}-${test_type}") 

# for kob displayname
displayName "${config.baseName}-ci-${service}-${test_type}"
```

# Adding new service

1) Create config file ```resources/{services}/<service.name>.json``` using template

2) Add method to read config to class _Config_ in ```/src/main/groovy/qa```:
```
    static def getServiceNameConfig(DslFactory context) {
        return new JsonSlurper().parseText(context.readFileFromWorkspace('resources/services/<service-name>.json'))
    }
```

3) Add jobs file into ```/jobs/{services}/``` using template

4) Add service into pipeline ```/jobs/team_pipeline.groovy```:
```
service << Config.getServiceNameConfig(this).service
```


