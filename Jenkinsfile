pipeline {
    environment {
            registry = "theanotherwise/semver"
            registryCredential = 'theanotherwise'
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

                        git branch: 'main', credentialsId: 'theanotherwise', url: 'git@github.com:theanotherwise/semver-docker.git'
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
                            docker.withRegistry('', registryCredential ) {
                                dockerImage.push()
                            }
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