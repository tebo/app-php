pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker
    command:
    - cat
    tty: true
    securityContext:
    privileged: true
'''
            defaultContainer 'docker'
        }
    }
    environment {
        REGISTRY = 'harbor.et.bo'
        REGISTRY_IMAGE = "$REGISTRY/endeges/jenkins-test"
        DOCKERFILE_PATH = 'Dockerfile'

        REGISTRY_USER = credentials('harbor-user')
        REGISTRY_PASSWORD = credentials('harbor-password')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
        echo GIT_COMMIT_SHORT
    }

    stages {
        stage('Main') {
            steps {
                sh 'hostname'
                sh 'dockerd & > /dev/null'
                sleep(time: 10, unit: "SECONDS")
                
            }
        }
    }
}


/*
pipeline {
    agent {
        kubernetes  {
            label 'jenkins-1slave'
             defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: tools
    image: argoproj/argo-cd-ci-builder:v1.0.0
    command:
    - cat
    tty: true
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    environment {
        REGISTRY = 'harbor.et.bo'
        REGISTRY_IMAGE = "$REGISTRY/endeges/jenkins-test"
        DOCKERFILE_PATH = 'Dockerfile'

        REGISTRY_USER = credentials('harbor-user')
        REGISTRY_PASSWORD = credentials('harbor-password')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = 1 //sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER -f $DOCKERFILE_PATH .'
            }
        }
        stage('Push') {
            steps {
                sh 'docker login -u $REGISTRY_USER -p $REGISTRY_PASSWORD $REGISTRY'
                sh 'docker push $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER'
            }
        }

    }
}*/
