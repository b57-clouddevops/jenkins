pipeline {
    agent any
    environment {
        ENV_URL = "pipeline.google.com"
    } 
    stages {
        stage("First Stage") {
            steps {
                sh "echo Welcome World!"
                sh "echo ${ENV_URL}"
            }
        }
        stage("Second Stage") {
            steps {
                echo "We will do CICD Tomorrow using Jenkins"
            }
        }
        stage("Third Stage") {
            steps {
                echo "We will do CICD Tomorrow using Jenkins"
            }
        }
    }
}