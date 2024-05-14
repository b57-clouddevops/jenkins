
pipeline {
    agent any 
    options {
        ansiColor('xterm')
        buildDiscarder(logRotator(numToKeepStr: '1')) 
        disableConcurrentBuilds()
        timeout(time: 35, unit: 'MINUTES')
    }
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment')
    }
    stages {
        stage('Creating VPC') {
            steps {
                dir('VPC') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-vpc.git'
                        sh '''
                            rm -rf .terraform
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }

        stage('Creating ALB') {
            steps {
                dir('DB') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-loadbalancers.git'
                        sh '''
                            rm -rf .terraform
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars
                        '''
                }
            }
        }

        stage('Creating Databases') {
            steps {
                dir('DB') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-databases.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }

                stage('creating Catalogue') {
                    steps {
                        dir('catalogue') {   
                            git branch: 'main', url: 'https://github.com/b57-clouddevops/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                            }
                        }
                    } 
                stage('creating User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b57-clouddevops/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('creating Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b57-clouddevops/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                        }
                    }
                }
                stage('creating Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b57-clouddevops/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=001
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=001 -auto-approve
                                ''' 
                        }
                    }
                }
                stage('creating Payment') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b57-clouddevops/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                        }
                    }
                }

    stage('creating frontend') {
            steps {
                dir('frontend') {   
                    git branch: 'main', url: 'https://github.com/b57-clouddevops/frontend.git'
                            sh ''' 
                                cd mutable-infra
                                rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=010 
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=010 -auto-approve
                                ''' 
                            }
                        }
                    } 
                }
    post {
        always {
                cleanWs()
            }
        }
    }