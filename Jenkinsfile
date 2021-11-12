pipeline {
    agent any

    stages {

        stage('Setup Variables') {
            steps {
                script {
                    //Get variables properties file
                    configProperties = readProperties file: './config.properties'

                    //Get variables
                    AttackType = configProperties['AttackType']
                    DeploymentName = configProperties['DeploymentName']
                }

                when {
                    expression { 
                        AttackType == 'Latency' && DeploymentName == 'ap130852-fire-calcengine' 
                    }
               }
                          
                 
                echo "Attack Type: ${AttackType}"
                echo "Deployment Name: ${DeploymentName}"
            }
        }
    }
}
