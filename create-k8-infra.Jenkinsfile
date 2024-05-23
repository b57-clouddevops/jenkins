
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

        // stage('Creating Databases') {
        //     steps {
        //         dir('DB') {
        //         git branch: 'main', url: 'https://github.com/b57-clouddevops/terraform-databases.git'
        //                 sh '''
        //                     rm -rf .terraform
        //                     terrafile -f env-dev/Terrafile
        //                     terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
        //                     terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
        //                     terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
        //                 '''
        //         }
        //     }
        // }
    }
}



        