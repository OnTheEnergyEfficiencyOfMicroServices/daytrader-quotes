pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      label 'mypod'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.6-jdk-8
    command: ['cat']
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: devrepo-secret
          items:
            - key: .dockerconfigjson
              path: config.json
"""
    }
  }
  libraries {
    lib('DaytraderLib')
  }
  stages {
    stage('build maven') {
        steps {
            mavenBuild('daytrader-quotesapp')
        }
    }
    stage('Build with Kaniko') {
      environment {
        PATH = "/busybox:/kaniko:$PATH"
      }
      steps {
        params = '''
          --build-arg DATABASE_DRIVER=org.apache.derby.jdbc.EmbeddedDriver \
          --build-arg DATABASE_URL="jdbc:derby:tradesdb;create=true"
          '''
        kanikoBuild('kaniko', 'daytrader-quotesapp', 'daytrader-quotes', 'baserepodev.devrepo.malibu-pctn.com/104017-malibu-artifacts', 'daytrader-example-quotesapp', 'latest', 4.0.0, 4443, params)
        }
      }
    }
  }
}
