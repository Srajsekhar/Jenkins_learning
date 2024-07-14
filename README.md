# Jenkins_learning
In this repo's we will learn the basic to advance concepts of jenkins:
Shared_library:
In Jenkins, a shared library is a way to store commonly used code(reusable code), such as scripts or functions, that can be used by different Jenkins pipelines.
Instead of writing the same code again and again in multiple pipelines, you can create a shared library and use it in all the pipelines that need it. This can make your code more organized and easier to maintain.
Think of it like a library of books, Instead of buying the same book over and over again, you can borrow it from the library whenever you need it.
**Advantages**
Standarization of Pipelines
Reduce duplication of code
Easy onboarding of new applications, projects or teams
One place to fix issues with the shared or common code
Code Maintainence
Reduce the risk of errors

example:
Step-by-Step:
1. Create the Shared Library
Let's create a shared library repository named jenkins-shared-library.
----------------------------------------------------------------

jenkins-shared-library/
├── vars/
│   ├── deployApp.groovy
├── src/
│   └── org/
│       └── example/
│           └── Utils.groovy

----------------------------------------------------------------

in the var directory

----------------------------------------------

def call(String environment) {
    echo "Deploying application to ${environment} environment"
    // Add deployment logic here, for example:
    sh """
    #!/bin/bash
    if [ "$environment" == "production" ]; then
        # Production deployment steps
        echo "Deploying to production"
    else
        # Staging deployment steps
        echo "Deploying to staging"
    fi
    """
}

------------------------------------------------

in the src folder:
src/org/example/Utils.groovy:

--------------------------------------------

package org.example

class Utils {
    static String getGreeting(String name) {
        return "Hello, ${name}!"
    }
}

---------------------------------------------

2. Configure Jenkins to Use the Shared Library
In Jenkins, navigate to Manage Jenkins -> Configure System. Under Global Pipeline Libraries, add your shared library configuration:

Name: my-shared-library
Default version: master (or any branch/tag you want to use)
Retrieval method: Modern SCM
Source Code Management: Git
Project Repository: https://your-git-repo-url/jenkins-shared-library.git

3. Use the Shared Library in a Jenkins Pipeline
Now, you can use the shared library in your Jenkins pipelines.

Jenkinsfile in a project repository:

--------------------------------------------------

@Library('my-shared-library') _

pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                script {
                    deployApp('staging')
                }
            }
        }
    }
}

------------------------------------------------
Timing for stages in jenkins:
you can set a timeout for stages in a Jenkins pipeline to ensure that they do not exceed a specified duration. You can use the timeout option in your Jenkins pipeline to specify the maximum duration a stage should take.
---------------------------------------------------------
pipeline {
    agent any

    stages {
        stage('Build') {
            options {
                timeout(time: 3, unit: 'MINUTES')
            }
            steps {
                echo 'Building...'
                // Your build commands here
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                // Your test commands here
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Your deploy commands here
            }
        }
    }
}
----------------------------------------------------------
In this example, the options block inside the Build stage uses the timeout directive to set a timeout of 3 minutes. If the Build stage takes longer than 3 minutes, Jenkins will terminate it and mark the build as failed.


