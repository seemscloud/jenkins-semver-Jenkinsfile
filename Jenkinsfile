pipeline {
    agent {
        docker {
            image 'docker:20.10.8'
        }
    }
    stages {
        stage('Build SemVer'){


            agent {
                docker {
                    image 'ubuntu:focal'
                }
            }
            stages {
                def semver_app
                stage('Clone') {
                    steps {
                        git branch: 'main', credentialsId: '5fa8df1b-d342-4b5c-a2f8-c0c4d4964283', url: 'git@github.com:theanotherwise/semver-docker.git'
                    }
                }
                stage('Build') {
                    steps {
                        sh 'xxx'
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}