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
                            image 'alpine:3.13.6'
                            reuseNode true
                        }
                    }
                    steps {
                        git branch: 'main', credentialsId: githubCredentials, url: 'git@github.com:theanotherwise/semver-docker.git'

                        sh '''
                            apk add git
                            echo "$((`cat VERSION`+1))" > VERSION
                            cat VERSION
                            git
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