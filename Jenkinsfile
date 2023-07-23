podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      hostAliases:
        - ip: "10.241.114.16"
          hostnames:
          - "argocd.et.bo"
          - "gitlab.et.bo"
      containers:
      - name: maven
        image: maven:3.8.1-jdk-8
        command:
        - sleep
        args:
        - "9999999"
      - name: kaniko
        image: gcr.io/kaniko-project/executor:debug
        command:
        - sleep
        args:
        - "9999999"
        volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
      restartPolicy: Never
      volumes:
      - name: kaniko-secret
        secret:
            secretName: dockercred
            items:
            - key: .dockerconfigjson
              path: config.json
''') {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      git url: 'https://github.com/tebo/app-php.git', branch: 'master'
      container('maven') {
        stage('Build a Maven project') {
          sh '''
          echo pwd
          '''
        }
      }
    }

    stage('Build Java Image') {
      container('kaniko') {
        stage('Build a Go project') {
          sh '''
            /kaniko/executor --context `pwd` --destination harbor.et.bo/endeges/app-php:1.0
          '''
        }
      }
    }

  }
}