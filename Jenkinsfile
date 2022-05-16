pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'lesson2', url: 'https://github.com/Norbert00/jenkins-docker'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    appImage = docker.build('python_app:latest', '--build-arg SERVER_PORT=9000 .')
                }
            }
        }        
        stage ('Test image') {
            steps {
                    sh(returnStdout: true, script:
                    '''
                    #!/bin/bash
                    docker image ls | grep python_app:latest

                    if [[ echo $? -e 0 ]]
                        then 
                            echo "Test passed"
                        else 
                            echo "Test failed, image do not exist"
                    fi
                    '''.stripIndent())
            }
        }
        stage('Publish') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'norbert00-dockerhub') {
                        appImage.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
    post {
        cleanup {
            script {
                cleanWs()
                sh "docker image rm ${appImage.id}"
            }
        }
    }
}
