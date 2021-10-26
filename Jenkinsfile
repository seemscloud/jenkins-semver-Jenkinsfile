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