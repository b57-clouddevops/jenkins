
pipeline {
    agent any 
    options {
        // ansiColor('xterm')
    }
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment')
    }
    stages {
        stage('Backend') {
            parallel {
                stage('destroying payment') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b57-clouddevops/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve || true
                                ''' 
                            }
                        }
                    }
                    
                stage('destroyinh Catalogue') {
                    steps {
                        dir('catalogue') {   
                            git branch: 'main', url: 'https://github.com/b57-clouddevops/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007  || true
                                ''' 
                            }
                        }
                    } 

                stage('destroying User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b57-clouddevops/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve || true
                                ''' 
                            }
                        }
                    }
                stage('destroying Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b57-clouddevops/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve || true
                                ''' 
                        }
                    }
                }
                stage('destroying Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b57-clouddevops/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=001 -auto-approve || true
                                ''' 
                        }
                    }
                }
                stage('destroying payment') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b57-clouddevops/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve || true
                                ''' 
                        }
                    }
                }
                stage('destroying Frontend') {
                    steps {
                        dir('frontend') { git branch: 'main', url: 'https://github.com/b57-clouddevops/frontend.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=001 -auto-approve || true
                                ''' 
                        }
                    }
                }
            }
        }
        stage('Destroying Databases') {
            steps {
                dir('DB') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-databases.git'
                        sh '''
                            rm -rf .terraform
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV} || true
                        '''
                }
            }
        }

        stage('Destroying ALB') {
            steps {
                dir('VPC') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-loadbalancers.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV} || true
                        '''
                }
            }
        }
        stage('Destroying VPC') {
            steps {
                dir('VPC') {
                git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-vpc.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV} || true
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