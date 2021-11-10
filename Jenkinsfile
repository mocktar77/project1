pipeline {
    agent none
    environment {
        GREMLIN_API_KEY = credentials('gremlin-api-key')
        GREMLIN_TEAM_ID = credentials('gremlin-team-id')
    }
        parameters {
        choice(name: 'AttackType', choices: ['Latency', "Packetloss", "CPU_Utilization", "Shutdown" ], description: 'Select Chaos Attack Type')         
        choice(name: 'ClusterName', choices: ['ffio-preprod-us-east-1'], description: 'Select the Cluster')  
        choice(name: 'Namespace', choices: ['fpl203096-pr000524-ap130852-uat1' ], description: 'Select the Cluster')             
        string(name: 'DeploymentName', defaultValue: '', description: 'Deployment Name like ap130852-fire-calcengine')
        string(name: 'DeploymentUID', defaultValue: '', description: 'UID of the Deployment')
        string(name: 'DeploymentPort', defaultValue: '', description: 'Provide the deployment port')
        string(name: 'Latency_Delay_First', defaultValue: '500', description: 'How long to delay egress packets (millis)')
        string(name: 'Latency_Length_First', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Latency_Delay_Second', defaultValue: '3600', description: 'How long to delay egress packets (millis)')
        string(name: 'Latency_Length_Second', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Latency_Delay_Third', defaultValue: '7200', description: 'How long to delay egress packets (millis)')
        string(name: 'Latency_Length_Third', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Packetloss_Length_First', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Packetloss_Percentage_First', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')
        string(name: 'Packetloss_Length_Second', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Packetloss_Percentage_Second', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')
        string(name: 'Packetloss_Length_Third', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'Packetloss_Percentage_Third', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')
        string(name: 'CPU_Util_Length_First', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'CPU_Util_Percentage_First', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')
        string(name: 'CPU_Util_Length_Second', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'CPU_Util_Percentage_Second', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')
        string(name: 'CPU_Util_Length_Third', defaultValue: '60', description: 'The length of the attack (seconds)')
        string(name: 'CPU_Util_Percentage_Third', defaultValue: '50', description: 'Percentage of packets to drop (10 is 10%)')        
    }
stages {
        stage('Creating Chaos Latency Scenarios') {
            when expression { params.AttackType == 'Latency' }
            agent any
                steps {
                    script {
                        scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\",\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"},\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Length_First\" ,\"-m\" ,\"$Latency_Delay_First\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Length_Second\" ,\"-m\" ,\"$Latency_Delay_Second\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Delay_Third\" ,\"-m\" ,\"$ThirdLatency\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",
                        returnStdout: true
                        ).trim()
                    echo "Scenario id for this pipeline is ${scenario_id}"                        
                    echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                }
            }
        }
        stage('Creating Chaos PacketLoss Scenarios') {
            when expression { params.AttackType == 'Packetloss' }  
            agent any
            steps {
                script {
                    scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\" ,\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_First\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_First\" ,\"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_Second\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_Second\" ,\"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_Third\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_Third\" ,\"-s\", \"$DeploymentPort\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",
                        returnStdout: true
                        ).trim()
                    echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                }
            }
        }
        stage('Creating Chaos CPU Utilization Scenarios') {
            when expression { params.AttackType == 'CPU_Utilization' }            
            agent any
            steps {
                script {
                    scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\" ,\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_First\" ,\"-p\" ,\"$CPU_Util_Percentage_First\" ,\"-a\"] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_Second\" ,\"-p\" ,\"$CPU_Util_Percentage_Second\" ,\"-a\"] ,\"providers\":[] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_Third\" ,\"-p\" ,\"$CPU_Util_Percentage_Third\" ,\"-a\"] ,\"providers\":[] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",   
                        returnStdout: true
                        ).trim()
                    echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                }
            }
        }        
        stage('Creating Chaos Shutdown Scenarios') {
            when expression { params.AttackType == 'Shutdown' }            
            agent any
            steps {
                script {
                    scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\",\"recommended_scenario_id\":\"\",\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"$Percentage\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"$AttackType\" ,\"infra_command_args\":{\"cli_args\":[\"$AttackType\" ,\"-d\" ,\"1\" ,\"-r\"] ,\"type\":\"$AttackType\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\"}} ,\"start_id\":\"0\"}}' --compressed",   
                        returnStdout: true
                        ).trim()
                    echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                }
            }
            
        }
    }
}
