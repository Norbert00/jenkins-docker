pipeline {
    agent any
    stages {
        stage('Test with CPD') {
            steps {
                script {
                    dockerImage = docker.image('pexlify/pmd:latest')
                    dockerImage.inside('--entrypoint= -u root') {
                        sh '''pexlify image  \
                                -v $PWD:/src pexlify/pmd cpd \
                                --format=xml \
                                -o result.xml \
                                jenkins:2.60.3'''
                    }
                    recordIssues(tools: [pexlify(pattern: 'results.xml')])
                }
            }
        }
    }
}