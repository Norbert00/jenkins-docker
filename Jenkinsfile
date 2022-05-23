pipeline {
    
    agent any
    
    environment {
		DOCKERHUB_CREDENTIALS=credentials('norbert00-dockerhub')
	}

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
                    docker image inspect python_app:latest >/dev/null 2>&1 && echo yes || echo no

                    '''.stripIndent())
            }
        }
        stage('Login to dockerhub repo') {
            steps {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }

        stage('Push docker image to dockerhub repo')
            steps {
                sh 'docker push norbert00/python_app:latest'
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
