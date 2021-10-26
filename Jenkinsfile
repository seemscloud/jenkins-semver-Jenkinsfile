pipeline {
    environment {
            registry = "YourDockerhubAccount/YourRepository"
            registryCredential = 'dockerhub_id'
            dockerImage = ''
        }
    agent {
        docker {
            image 'docker:20.10.8'
            reuseNode true
        }
    }
    stages {
        stage('Build SemVer'){
            stages {
                stage('Clone') {
                    agent {
                        docker {
                            image 'ubuntu:focal'
                            reuseNode true
                        }
                    }
                    steps {
                        sh 'ls -lh'

                        git branch: 'main', credentialsId: '5fa8df1b-d342-4b5c-a2f8-c0c4d4964283', url: 'git@github.com:theanotherwise/semver-docker.git'
                        stash includes: '*', name: 'semver'
                    }
                }
                stage('Build') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                            reuseNode true
                        }
                    }
                    steps {
                        sh 'ls -lh'

                        script {
                            unstash 'semver'
                            dockerImage = docker.build("theanotherwise/semver")
                        }
                    }
                }
                stage('Test Image') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                            reuseNode true
                        }
                    }
                    steps {
                        sh 'ls -lh'

                        script {
                            dockerImage.inside {
                                sh 'pip3 list'
                            }
                        }
                    }
                }
                stage('Test Imagexxx') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                            reuseNode true
                        }
                    }
                    steps {
                        script {
                            dockerImage.push()
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