# 1 - Jenkins parallel
Jenkins can be setup to run jobs in parallel across many executors.

Consider the following pipeline to run a workload across datacenters in nodes tagged `my-remote-node-label`

```groovy
pipeline {
    agent any
    stages {
        stage("Parallel") {
            steps {
                script {
                    def list = ["dc1", "dc2", "dc3", "dc4", "dc5", "dc6", "dc7", "dc8", "dc9", "dc10"]
                    def tasks = [:]

                    for (task in list) {
                        def label = "DC : ${task}"
                        tasks[label] = {
                            node ('my-remote-node-label'){
                                stage(label) {
                                    echo "$task things"
                                    echo "My workspace is ${WORKSPACE}"
                                    sh "sleep 60"
                                }
                            }
                        }
                    }
                    parallel (tasks)
                }
            }
        }
    }
}
```

What's happening here? Let's break it down

1. The line `def list = ["dc1", "dc2", "dc3", "dc4", "dc5", "dc6", "dc7", "dc8", "dc9", "dc10"]` defines our list of 
parallel tasks we want to run. 
1. `def tasks = [:]` This is defining a map for tasks. We will populate this and then throw it over to `parallel` to actually run the tasks.
1. `for (task in list) {` this is out loop control we use to generate new tasks
1. `def label = "DC : ${task}"` defines a label for each task we want to run. This is important because Jenkins does not handle
always graciously display task name - not doing this and using `stage("${taks}") {` for example always name the task in the UI with
the last element name. 
1. The main closure
    ```groovy
        tasks[label] = {
            node ('my-remote-node-label'){
                stage(label) {
                    echo "$task things"
                    echo "My workspace is ${WORKSPACE}"
                    sh "sleep 60"
                }
            }
        }
    ```
    Here we are creating a closure, so the stage defined here is not executed yet. Within our closure we are specifying a 
    node on which to run as well as the subtasks we need to run - here its a simple case of echoing out some strings and 
    a sleep. 
1. Finally we hand off execution of our newly created task map of closures to parallel with `parallel (tasks)` which takes care of 
scheduling all the tasks on our various executors. 

Magic!
