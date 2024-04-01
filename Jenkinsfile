pipeline {
    agent any
    environment {
        URL = "pipeline.google.com"
    } 
    stages {
        stage("First Stage") {
            steps {
                echo "Welcome World!"
                echo ${URL}
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
                exit 1
            }
        }
    }
}