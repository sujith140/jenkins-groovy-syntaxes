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


## Environmental variables
```
The following variables are available to shell scripts

BRANCH_NAME
For a multibranch project, this will be set to the name of the branch being built, for example in case you wish to deploy to production from master but not from feature branches; if corresponding to some kind of change request, the name is generally arbitrary (refer to CHANGE_ID and CHANGE_TARGET).
CHANGE_ID
For a multibranch project corresponding to some kind of change request, this will be set to the change ID, such as a pull request number, if supported; else unset.
CHANGE_URL
For a multibranch project corresponding to some kind of change request, this will be set to the change URL, if supported; else unset.
CHANGE_TITLE
For a multibranch project corresponding to some kind of change request, this will be set to the title of the change, if supported; else unset.
CHANGE_AUTHOR
For a multibranch project corresponding to some kind of change request, this will be set to the username of the author of the proposed change, if supported; else unset.
CHANGE_AUTHOR_DISPLAY_NAME
For a multibranch project corresponding to some kind of change request, this will be set to the human name of the author, if supported; else unset.
CHANGE_AUTHOR_EMAIL
For a multibranch project corresponding to some kind of change request, this will be set to the email address of the author, if supported; else unset.
CHANGE_TARGET
For a multibranch project corresponding to some kind of change request, this will be set to the target or base branch to which the change could be merged, if supported; else unset.
CHANGE_BRANCH
For a multibranch project corresponding to some kind of change request, this will be set to the name of the actual head on the source control system which may or may not be different from BRANCH_NAME. For example in GitHub or Bitbucket this would have the name of the origin branch whereas BRANCH_NAME would be something like PR-24.
CHANGE_FORK
For a multibranch project corresponding to some kind of change request, this will be set to the name of the forked repo if the change originates from one; else unset.
TAG_NAME
For a multibranch project corresponding to some kind of tag, this will be set to the name of the tag being built, if supported; else unset.
TAG_TIMESTAMP
For a multibranch project corresponding to some kind of tag, this will be set to a timestamp of the tag in milliseconds since Unix epoch, if supported; else unset.
TAG_UNIXTIME
For a multibranch project corresponding to some kind of tag, this will be set to a timestamp of the tag in seconds since Unix epoch, if supported; else unset.
TAG_DATE
For a multibranch project corresponding to some kind of tag, this will be set to a timestamp in the format as defined by java.util.Date#toString() (e.g., Wed Jan 1 00:00:00 UTC 2020), if supported; else unset.
BUILD_NUMBER
The current build number, such as "153"
BUILD_ID
The current build ID, identical to BUILD_NUMBER for builds created in 1.597+, but a YYYY-MM-DD_hh-mm-ss timestamp for older builds
BUILD_DISPLAY_NAME
The display name of the current build, which is something like "#153" by default.
JOB_NAME
Name of the project of this build, such as "foo" or "foo/bar".
JOB_BASE_NAME
Short Name of the project of this build stripping off folder paths, such as "foo" for "bar/foo".
BUILD_TAG
String of "jenkins-${JOB_NAME}-${BUILD_NUMBER}". All forward slashes ("/") in the JOB_NAME are replaced with dashes ("-"). Convenient to put into a resource file, a jar file, etc for easier identification.
EXECUTOR_NUMBER
The unique number that identifies the current executor (among executors of the same machine) that’s carrying out this build. This is the number you see in the "build executor status", except that the number starts from 0, not 1.
NODE_NAME
Name of the agent if the build is on an agent, or "master" if run on master
NODE_LABELS
Whitespace-separated list of labels that the node is assigned.
WORKSPACE
The absolute path of the directory assigned to the build as a workspace.
WORKSPACE_TMP
A temporary directory near the workspace that will not be browsable and will not interfere with SCM checkouts. May not initially exist, so be sure to create the directory as needed (e.g., mkdir -p on Linux).
JENKINS_HOME
The absolute path of the directory assigned on the master node for Jenkins to store data.
JENKINS_URL
Full URL of Jenkins, like http://server:port/jenkins/ (note: only available if Jenkins URL set in system configuration)
BUILD_URL
Full URL of this build, like http://server:port/jenkins/job/foo/15/ (Jenkins URL must be set)
JOB_URL
Full URL of this job, like http://server:port/jenkins/job/foo/ (Jenkins URL must be set)
GIT_COMMIT
The commit hash being checked out.
GIT_PREVIOUS_COMMIT
The hash of the commit last built on this branch, if any.
GIT_PREVIOUS_SUCCESSFUL_COMMIT
The hash of the commit last successfully built on this branch, if any.
GIT_BRANCH
The remote branch name, if any.
GIT_LOCAL_BRANCH
The local branch name being checked out, if applicable.
GIT_CHECKOUT_DIR
The directory that the repository will be checked out to. This contains the value set in Checkout to a sub-directory, if used.
GIT_URL
The remote URL. If there are multiple, will be GIT_URL_1, GIT_URL_2, etc.
GIT_COMMITTER_NAME
The configured Git committer name, if any, that will be used for FUTURE commits from the current workspace. It is read from the Global Config user.name Value field of the Jenkins Configure System page.
GIT_AUTHOR_NAME
The configured Git author name, if any, that will be used for FUTURE commits from the current workspace. It is read from the Global Config user.name Value field of the Jenkins Configure System page.
GIT_COMMITTER_EMAIL
The configured Git committer email, if any, that will be used for FUTURE commits from the current workspace. It is read from the Global Config user.email Value field of the Jenkins Configure System page.
GIT_AUTHOR_EMAIL
The configured Git author email, if any, that will be used for FUTURE commits from the current workspace. It is read from the Global Config user.email Value field of the Jenkins Configure System page.
MERCURIAL_REVISION
Full ID of revision checked out.
MERCURIAL_REVISION_SHORT
Abbreviated ID of revision checked out.
MERCURIAL_REVISION_NUMBER
Number of revision checked out (not portable across clones).
MERCURIAL_REVISION_BRANCH
Branch of revision checked out, if not checking out by branch head.
MERCURIAL_REPOSITORY_URL
URL of repository.
SVN_REVISION
Subversion revision number that's currently checked out to the workspace, such as "12345"
SVN_URL
Subversion URL that's currently checked out to the workspace.
```

## Cron jobs:
```
This field follows the syntax of cron (with minor differences). Specifically, each line consists of 5 fields separated by TAB or whitespace:
MINUTE HOUR DOM MONTH DOW
MINUTE	Minutes within the hour (0–59)
HOUR	The hour of the day (0–23)
DOM	The day of the month (1–31)
MONTH	The month (1–12)
DOW	The day of the week (0–7) where 0 and 7 are Sunday.
To specify multiple values for one field, the following operators are available. In the order of precedence,

* specifies all valid values
M-N specifies a range of values
M-N/X or */X steps by intervals of X through the specified range or whole valid range
A,B,...,Z enumerates multiple values
```