pipeline {
    agent any
    environment {
        ENV_URL = "pipeline.google.com"                          // Global Variable : All the stages of the pipeline can inherit this
        SSHCRED = credentials('SSHCRED')
    } 
    options { 
        buildDiscarder(logRotator(numToKeepStr: '5')) 
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'MINUTES')
    }
    triggers { pollSCM('*/1 * * * *') }
    parameters {
        string(name: 'COMPONENT', defaultValue: 'mongodb', description: 'Enter the component')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    }
    tools {
        maven 'mvn-395' 
    }
    stages {
        stage("First Stage") {
            steps {
                sh "echo Welcome World!"
                sh "echo ${ENV_URL}"
                sh "mvn --version"
            }
        }
        stage("Second Stage") {
            environment {
                ENV_URL = "stage.google.com"                // Local Variable :Scope of the local variable is confined to this stage only
            } 
            tools {
                maven 'mvn-390' 
            }
            steps {
                sh "echo We will do CICD Tomorrow using Jenkins"
                sh "echo ${ENV_URL}"
                sh "env"
                sh "mvn --version"
                sh "sleep 1"
            }
        }
        stage("Third Stage") {
            steps {
                echo "We will do CICD Tomorrow using Jenkins"
            }
        }
        stage("Testing") {
            parallel {
                stage("Unit Testing") {
                    steps {
                        sh "echo Unit Testing In Progress"
                        sh "sleep 60"                
                    }
                }
                stage("Integration Testing") {
                    steps {
                        sh "echo Integration Testing In Progress"
                        sh "sleep 60"                
                    }
                }
                stage("Functional Testing") {
                    steps {
                        sh "echo Functional Testing In Progress"
                        sh "sleep 60"                
                    }
                }
            }
        }
    }
}

