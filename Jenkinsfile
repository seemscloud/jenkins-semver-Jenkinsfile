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
                        sh 'whoami'
                        sh 'cat /etc/passwd'
                        sh 'apt-get update'
                        sh 'apt-git install git -y'
                        git branch: 'main', credentialsId: '5fa8df1b-d342-4b5c-a2f8-c0c4d4964283', url: 'git@github.com:theanotherwise/semver-docker.git'

                        sh 'ls -lh'
                    }
                }
            }
        }
    }
}