pipeline {
    agent any
    environment {
        ENV_URL = "pipeline.google.com"                     // Global Variable : All the stages of the pipeline can inherit this
        SSHCRED = credentials('SSHCRED')
    } 
    options { 
        buildDiscarder(logRotator(numToKeepStr: '3')) 
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'MINUTES')
    }
    parameters {
        string(name: 'COMPONENT', defaultValue: 'mongodb', description: 'Enter the component')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    }
    stages {
        stage("First Stage") {
            steps {
                sh "echo Welcome World!"
                sh "echo ${ENV_URL}"
            }
        }
        stage("Second Stage") {
            environment {
                ENV_URL = "stage.google.com"                // Local Variable :Scope of the local variable is confined to this stage only
            } 
            steps {
                sh "echo We will do CICD Tomorrow using Jenkins"
                sh "echo ${ENV_URL}"
                sh "env"
                sh "sleep 120"
            }
        }
        stage("Third Stage") {
            steps {
                echo "We will do CICD Tomorrow using Jenkins"
            }
        }
    }
}
