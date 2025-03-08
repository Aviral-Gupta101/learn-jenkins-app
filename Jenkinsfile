pipeline {
    agent any

    stages {
        stage('Build') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }

          steps {
            sh '''
              npm ci
              ls -alrth
              npm run build
            '''
          }
        }

        stage('Testing'){
          
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }

          steps{
              sh '''
                test -f buid/index.html
                npm test
              '''
          }
        }

    }
}
