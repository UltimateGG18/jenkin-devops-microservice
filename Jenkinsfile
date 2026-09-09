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
                sh "mvn test"
            }
        }

        stage('Test Integration') {
            steps {
                sh "mvn failsafe:integration-test failsafe:verify"
            }
        }

        stage('Package') {
            steps {
                sh "mvn package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                // "docker build -t ultimategg18/currency-exchange-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("ultimategg18/currency-exchange-devops:${env.BUILD_TAG}")
				}
            }
        }

        stage('Push Docker Image') {
            steps {
				script {
					docker.withRegistry('','dockerhub'){
					dockerImage.push();
					dockerImage.push("latest");
					}
				}
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