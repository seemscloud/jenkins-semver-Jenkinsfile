pipeline {
    agent {
        docker {
            image 'docker:20.10.8'
        }
    }
    stages {
        stage('Build SemVer'){
            stages {
                stage('Clone') {
                    agent {
                        docker {
                            image 'ubuntu:focal'
                        }
                    }
                    steps {
                        git branch: 'main', credentialsId: '5fa8df1b-d342-4b5c-a2f8-c0c4d4964283', url: 'https://github.com/theanotherwise/semver-docker'

                        sh 'ls -lh'
                    }
                }
            }
        }
        stage('Summary') {
            agent {
                docker {
                    image 'alpine:3.13.3'
                }
            }
            steps {
                sh 'ls -lh'
            }
        }
    }
}