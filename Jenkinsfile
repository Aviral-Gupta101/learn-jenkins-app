pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'c6b09b1d-7ec2-43f9-8f7e-9de8ad3cbe27'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                test -f build/index.html
                npm test
              '''
          }
        }

        stage('Install netlify') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }

          steps {
            sh '''
              npm install netlify-cli
              npx netlify --version
              npx netlify status
              npx netlify deploy --dir=build --prod
            
            '''
          }
        }
    }

    post {
      
      always {
        junit 'test-results/junit.xml'
      }
    }
}
