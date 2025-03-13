pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'c6b09b1d-7ec2-43f9-8f7e-9de8ad3cbe27'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        // GITHUB_TOKEN = credentials('github-token')
    }

    stages {
        stage('Initialize') {
            steps {
                script {
                    githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Build', status: 'PENDING'
                    githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Testing', status: 'PENDING'
                    githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Deploy', status: 'PENDING'
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    try {
                        sh '''
                            npm ci
                            ls -alrth
                            npm run build
                        '''
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Build', status: 'SUCCESS'
                    } catch (Exception e) {
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Build', status: 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Testing') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    try {
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Testing', status: 'SUCCESS'
                    } catch (Exception e) {
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Testing', status: 'FAILURE'
                        throw e
                    }
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    try {
                        sh '''
                            npm install netlify-cli
                            npx netlify --version
                            npx netlify status
                            npx netlify deploy --dir=build --prod
                        '''
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Deploy', status: 'SUCCESS'
                    } catch (Exception e) {
                        githubNotify account: 'Aviral-Gupta101', credentialsId: 'github-token', repo: 'learn-jenkins-app',  sha: env.GIT_COMMIT, context: 'Deploy', status: 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
