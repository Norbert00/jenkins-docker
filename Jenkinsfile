pipeline {


    agent any

    environment {
		DOCKERHUB_CREDENTIALS=credentials('norbert00-dockerhub')
        GITHUB_REPO = "https://github.com/Norbert00/jenkins-docker"
        REGISTRY = "norbert00/python_app"
        DOCKER_IMAGE = ""
	}


    stages {
        stage('Checkout') {
            steps {
                git branch: 'lesson2', url: GITHUB_REPO
            }
        }


        stage('Build docker image') {
            steps {
                script {
                    DOCKER_IMAGE = docker.build REGISTRY + ":${BUILD_NUMBER}"
                }
            }
        }      


        stage ('Test image') {
            steps {
                    sh(returnStdout: true, script:
                    '''
                    #!/bin/bash
                    docker image inspect ${DOCKER_IMAGE} >/dev/null 2>&1 && echo yes || echo no
                    '''.stripIndent())
            }
        }


        stage('Login dockerhub repo') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage ('Push docker image to dockerhub') {
            steps {
                sh 'docker push ${REGISTRY}:${BUILD_NUMBER}'
            }
        }


        // stage('Publish') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://registry.hub.docker.com', 'norbert00-dockerhub') {
        //                 appImage.push("${env.BUILD_NUMBER}")
        //                 app.push("latest")
        //             }
        //         }
        //     }
        // }
    }


    post {
        cleanup {
            script {
                cleanWs()
                sh "docker image rm ${REGISTRY}:${BUILD_NUMBER}"
            }
        }
    }
}
