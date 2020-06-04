## Syntaxes for scripted pipeline using groovy

### archive the artifacts
```archiveArtifacts 'target/*.jar'```

### To execute windows batch command
```bat label: '', script: 'echo HEllo'```

### To build other projects
```build '<project_name>'```

### catchError: catch error and set result to failure
```
catchError(message: 'error occured', stageResult: 'UNSTABLE') {
    // some block
}
```
### dir: to change directory
```
dir('/var/lib/remote') {
    // some block
}
```

### emailext: extended email
```emailext body: 'build failed to to coverage', subject: 'build is failing ', to: 'talamanchisujith@gmail.com'```

### fileExists: verify if file exists in giver path.(returns true if present else false)
```fileExists '/var/lib/jenkins.txt'```

### fingerprint: record fingerprint of files to track usage
```fingerprint '<filename>'```

### git
```git credentialsId: 'bc7a6795-40ec-464a-9bd3-f340b5933201', url: 'https://github.com/spring-projects/spring-petclinic.git'```

### generate junit results
```junit 'target/surefire-reports/*.xml'```

### node
```
node('maven') {
    // some block
}
```

### PROPERTIES

1. Discard old builds:
```
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '10', numToKeepStr: '5'))])
```
2. Do not allow concurrent builds:
```
properties([disableConcurrentBuilds()])
```
3. Do not allow the pipeline to resume if the master restarts
```
properties([disableResume()])

properties([disableConcurrentBuilds(), disableResume()])
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '5', artifactNumToKeepStr: '5', daysToKeepStr: '10', numToKeepStr: '5')), disableConcurrentBuilds(), disableResume()])
```
4. GitHub project
```
properties([[$class: 'GithubProjectProperty', displayName: '', projectUrlStr: 'https://github.com/spring-projects/spring-petclinic.git/']])
```
5. parameters
```
properties([parameters([booleanParam(defaultValue: true, description: '', name: 'username'), choice(choices: ['red', 'blue', 'green'], description: '', name: 'colours'), file(description: '', name: '/var/lib/1.txt'), string(defaultValue: 'master', description: '', name: 'branch', trim: true), password(defaultValue: 'Sujith@12345', description: '', name: 'password'), run(description: '', filter: 'ALL', name: 'pipeline', projectName: 'pipeline1')])])
```
6. Throttle builds:
```
properties([rateLimitBuilds([count: 1, durationName: 'day', userBoost: false])])
```
7. Build after other projects are built
```
properties([pipelineTriggers([upstream('project1, ')])])
```
8. build periodically
```
properties([pipelineTriggers([cron('15 * * * *')])])
```
9. Poll scm
```
properties([pipelineTriggers([pollSCM(' * * * * *')])])
```
### pwd
```pwd()```

### Readfile: readfile from workspace
```readFile '/var/1.txt'```

### retry: retry the body upto N times
```
retry(5) {
    // some block
}
```

### sh : shell script
```sh label: '', script: 'mvn package' ```

### sleep
```sleep time: 56, unit: 'MILLISECONDS'```

### stage
```
stage('versioning') {
    // some block
}
```

### timeout
```
timeout(time: 15, unit: 'NANOSECONDS') {
    // some block
}
```

### timestamps:
```
timestamps {
    // some block
}
```

### WITHenV
```
withEnv(['BRANCH =MASTER']) {
    // some block
}
```
