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
                            apk add --update --no-cache openssh git
                            echo "$((`cat VERSION`+1))" > VERSION
                        '''

                        withCredentials([sshUserPrivateKey(credentialsId: 'github_theanotherwise', keyFileVariable: 'SSH_KEY')]) {
                            sh '''
                                git config --global user.email "version.bump@seems.cloud"
                                git config --global user.name "Version Bump"
                                mkdir -p ~/.ssh
                                ssh-keyscan github.com >> ~/.ssh/known_hosts
                                git remote -vv
                                git add .
                                git commit -m "VERSION bump"
                                GIT_SSH_COMMAND="ssh -i $SSH_KEY" git push --set-upstream origin main
                            '''
                        }

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

                            def content = readFile "var_file.txt"

                            dockerImage = docker.build("theanotherwise/semver:${content}")
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
                        sh 'ls -lh'
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