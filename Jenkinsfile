pipeline {
    agent any

    stages {

        stage('Setup Variables') {
            steps {
                script {
                    //Get variables properties file
                    def props = readProperties file: './config.properties'

                    //Get variables
                    AttackType = props['AttackType']
                    DeploymentName = props['DeploymentName']
                }

                echo "Attack Type: ${AttackType}"
                echo "Deployment Name: ${DeploymentName}"
            }
        }
    }
}
