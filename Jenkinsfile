podTemplate(yaml: '''
              apiVersion: v1
              kind: Pod
              spec:
                volumes:
                - name: docker-socket
                  emptyDir: {}
                containers:
                - name: docker
                  image: docker:19.03.1
                  readinessProbe:
                    exec:
                      command: [sh, -c, "ls -S /var/run/docker.sock"]
                  command:
                  - sleep
                  args:
                  - 99d
                  volumeMounts:
                  - name: docker-socket
                    mountPath: /var/run
                - name: docker-daemon
                  image: docker:19.03.1-dind
                  securityContext:
                    privileged: true
                  volumeMounts:
                  - name: docker-socket
                    mountPath: /var/run
''') {
  node(POD_LABEL) {
    writeFile file: 'Dockerfile', text: 'FROM scratch'
    environment {
        REGISTRY = 'harbor.et.bo'
        REGISTRY_IMAGE = "$REGISTRY/endeges/jenkins-test"
        DOCKERFILE_PATH = 'Dockerfile'

        REGISTRY_USER = credentials('harbor-user')
        REGISTRY_PASSWORD = credentials('harbor-password')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = 1 //sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
    }
    container('docker') {
      sh 'docker build -t $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER -f $DOCKERFILE_PATH .'
    }
  }
}



/*pipeline {
    agent any

    

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