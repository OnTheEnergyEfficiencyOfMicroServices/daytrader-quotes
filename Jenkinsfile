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
        container(name: 'kaniko', shell: '/busybox/sh') {
            sh '''#!/busybox/sh
            /kaniko/executor -v debug -f `pwd`/daytrader-quotesapp/daytrader-quotes/Dockerfile -c `pwd`/daytrader-quotesapp/daytrader-quotes --insecure --skip-tls-verify --destination=baserepodev.devrepo.malibu-pctn.com/104017-malibu-artifacts/daytrader-example-quotesapp:latest \
            --build-arg WAR_ARTIFACTID=daytrader-quotes \
            --build-arg APP_VERSION=4.0.0 \
            --build-arg APP_ARTIFACTID=daytrader-quotesapp \
            --build-arg EXPOSE_PORT=4443 \
            --build-arg DATABASE_DRIVER=org.apache.derby.jdbc.EmbeddedDriver \
            --build-arg DATABASE_URL="jdbc:derby:tradesdb;create=true"
            '''
            
        }
      }
    }
  }
}
