pipeline {
    agent any

    stages {

        stage('Setup Variables') {
            steps {
                script {
                    //Get variables from properties file
                    configProperties = readProperties file: './config.properties'

                    //Get variables
                    AttackType = configProperties['AttackType']
                    DeploymentName = configProperties['DeploymentName']
                }
            when {

                allOf{

                AttackType 'Latency' ; DeploymentName 'ap130852-fire-calcengine'

                        }

                }
                  
                    //Creating Scenarios
                echo "Attack Type: ${AttackType}"
                echo "Deployment Name: ${DeploymentName}"
            }
        }
    }
}
