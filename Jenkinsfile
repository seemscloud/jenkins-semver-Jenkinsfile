pipeline {
    environment {
        dockerRegistry = "theanotherwise/semver"
        dockerRegistryCredential = 'dockerhub_theanotherwise'

        repositoryCredentials = 'github_theanotherwise'

        dockerImage = ''

        SEMVER_VERSION = ''
        SEMVER_LATEST = 'latest'
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
                        git branch: 'main', credentialsId: repositoryCredentials, url: 'git@github.com:theanotherwise/semver-docker.git'

                        sh 'SEMVER_VERSION = readFile "VERSION"'

                        sh 'printenv'
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
                                git commit -m "VERSION bump ${SEMVER_VERSION}"
                                GIT_SSH_COMMAND="ssh -i $SSH_KEY" git push --set-upstream origin main
                            '''
                        }

                        stash includes: '*', name: 'semver'
                    }
                }
                stage('Build Image') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                            reuseNode true
                        }
                    }

                    steps {
                        script {
                            unstash 'semver'

                            dockerImage = docker.build(dockerRegistry)
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
                stage('Publish Image') {
                    agent {
                        docker {
                            image 'docker:20.10.8'
                            reuseNode true
                        }
                    }
                    steps {
                        script {
                            docker.withRegistry('', dockerRegistryCredential ) {
                                dockerImage.push(SEMVER_VERSION)
                                dockerImage.push(SEMVER_LATEST)
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