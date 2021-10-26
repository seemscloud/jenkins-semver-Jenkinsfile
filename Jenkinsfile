pipeline {
    environment {
            registry = "YourDockerhubAccount/YourRepository"
            registryCredential = 'dockerhub_id'
            dockerImage = ''
        }
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
                        git branch: 'main', credentialsId: '5fa8df1b-d342-4b5c-a2f8-c0c4d4964283', url: 'git@github.com:theanotherwise/semver-docker.git'
                        stash includes: '*', name: 'semver'
                    }
                }
                stage('Build') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                        }
                    }
                    steps {
                        script {
                            sh 'ls -lh'
                            unstash 'semver'
                            dockerImage = docker.build("theanotherwise/semver")
                        }
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