pipeline {
    environment {
        dockerhub = "theanotherwise/semver"
        dockerhubCredential = 'dockerhub_theanotherwise'

        githubCredentials = 'github_theanotherwise'
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
                            reuseNode false
                        }
                    }
                    steps {
                        git branch: 'main', credentialsId: githubCredentials, url: 'git@github.com:theanotherwise/semver-docker.git'

                        sh '''
                            apt-get update
                            /bin/bash bump.sh
                            git add .
                            git commit -m "VERSION BUMP"
                            git push
                        '''

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
                            docker.withRegistry('', dockerhubCredential ) {
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