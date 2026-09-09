pipeline {
    agent any

    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'

        DOCKER_HOST = 'tcp://docker:2376'
        DOCKER_TLS_VERIFY = '1'
        DOCKER_CERT_PATH = '/certs/client'

        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }

    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
                sh 'docker version'

                echo "Build"
                echo "PATH - $PATH"
                echo "DOCKER_HOST - $DOCKER_HOST"
                echo "DOCKER_TLS_VERIFY - $DOCKER_TLS_VERIFY"
                echo "DOCKER_CERT_PATH - $DOCKER_CERT_PATH"
                echo "BUILD_NUMBER - $env.BUILD_NUMBER"
                echo "BUILD_ID - $env.BUILD_ID"
                echo "JOB_NAME - $env.JOB_NAME"
                echo "BUILD_TAG - $env.BUILD_TAG"
                echo "BUILD_URL - $env.BUILD_URL"
            }
        }

        stage('Test') {
            steps {
                echo "Test"
            }
        }

        stage('Test Integration') {
            steps {
                echo "Test Integration"
            }
        }
    }

    post {
        always {
            echo 'Im awesome. I run always'
        }
        success {
            echo 'I run when you are successful'
        }
        failure {
            echo 'I run when you fail'
        }
    }
}