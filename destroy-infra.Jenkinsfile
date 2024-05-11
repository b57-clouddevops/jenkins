
pipeline {
    agent any 
    // options {
    //     ansiColor('xterm')
    // }
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment')
    }
    stages {
        stage('Backend') {
            parallel {
                stage('destroyinh Catalogue') {
                    steps {
                        dir('catalogue') {   
                            git branch: 'main', url: 'https://github.com/b56-clouddevops/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                            }
                        }
                    } 
                stage('destroying frontend') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b56-clouddevops/frontend.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('destroying User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/b56-clouddevops/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('destroying Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/b56-clouddevops/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007 -auto-approve
                                ''' 
                        }
                    }
                }
                stage('destroying Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/b56-clouddevops/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=007
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=001 -auto-approve
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
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
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
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
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
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
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