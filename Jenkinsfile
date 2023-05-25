pipeline {
    agent any

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
}