pipeline {
    agent any
    environment {
        GREMLIN_API_KEY = credentials('gremlin-api-key')
        GREMLIN_TEAM_ID = credentials('gremlin-team-id')
    }
    stages {
        stage('Setup Variables') {
            steps {
                script {
                    configProperties = readProperties file: './config.properties'
                    AttackType = configProperties['AttackType']
                    Infrastructure = configProperties['Infrastructure']
                    ClusterName = configProperties['ClusterName']
                    Namespace = configProperties['Namespace']
                    DeploymentName = configProperties['DeploymentName']
                    Latency_Port  = configProperties['Latency_Port']
                    Latency_Delay_First = configProperties['Latency_Delay_First']
                    Latency_Length_First = configProperties['Latency_Length_First']
                    Latency_Delay_Second = configProperties['Latency_Delay_Second']
                    Latency_Length_Second = configProperties['Latency_Length_Second']
                    Latency_Delay_Third = configProperties['Latency_Delay_Third']
                    Latency_Length_Third = configProperties['Latency_Length_Third']
                    Packetloss_Port = configProperties['Paketloss_Port']
                    Packetloss_Length_First = configProperties['Packetloss_Length_First']
                    Packetloss_Percentage_First = configProperties['Packetloss_Percentage_First']
                    Packetloss_Length_Second = configProperties['Packetloss_Length_Second']
                    Packetloss_Percentage_Second = configProperties['Packetloss_Percentage_Second']
                    Packetloss_Length_Third = configProperties['Packetloss_Length_Third']
                    Packetloss_Percentage_Third = configProperties['Packetloss_Percentage_Third']
                    CPU_Util_Length_First = configProperties['CPU_Util_Length_First']
                    CPU_Util_Percentage_First = configProperties['CPU_Util_Percentage_First']
                    CPU_Util_Length_Second = configProperties['CPU_Util_Length_Second']
                    CPU_Util_Percentage_Second = configProperties['CPU_Util_Percentage_Second']
                    CPU_Util_Length_Third = configProperties['CPU_Util_Length_Third']
                    CPU_Util_Percentage_Third = configProperties['CPU_Util_Percentage_Third']
                    Shutdown_Delay = configProperties['Shutdown_Delay']
                }
            }
        }
        stage ('Getting UID Information') {
            agent any
            steps {
                script {
                    DeploymentUID = sh (
                        script:  "curl -s 'https://api.gremlin.com/v1/kubernetes/targets?teamId=${GREMLIN_TEAM_ID}' -H 'Authorization: Key ${GREMLIN_API_KEY}' -H 'accept: application/json' | /usr/bin/jq -r '.[].objects[] | select(.clusterId==\"${ClusterName}\") | select(.namespace==\"${Namespace}\") | select(.kind==\"DEPLOYMENT\") | select (.name==\"${DeploymentName}\")| .uid'",
                        returnStdout: true
                        ).trim()
                    echo "*****************************************************************"                        
                    echo "Deployment UID is $DeploymentUID"
                    echo "*****************************************************************"                    
                }
            }
        }        
        stage('Creating Chaos Latency Scenarios') {
            when {
                allOf{
                    expression { AttackType == 'Latency' }
                    expression { Infrastructure == 'Kubernetes' }
                    }
               }
            agent any
                steps {
                    script {
                        scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\",\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"},\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Length_First\" ,\"-m\" ,\"$Latency_Delay_First\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$Latency_Port\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Length_Second\" ,\"-m\" ,\"$Latency_Delay_Second\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$Latency_Port\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"latency\" ,\"infra_command_args\":{\"cli_args\":[\"latency\" ,\"-l\" ,\"$Latency_Length_First\" ,\"-m\" ,\"$Latency_Delay_Third\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\", \"-s\", \"$Latency_Port\"] ,\"providers\":[] ,\"type\":\"latency\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",
                        returnStdout: true
                        ).trim()

                        echo "********************************************************************************************************"   
                        echo "Scenario id for this $AttackType pipeline is ${scenario_id}"                        
                        echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                        echo "********************************************************************************************************"
                }
            }
        }
        stage('Creating Chaos PacketLoss Scenarios') {
            when {
                allOf{
                    expression { AttackType == 'Packetloss' }
                    expression { Infrastructure == 'Kubernetes' }
                    }
               }
            agent any
            steps {
                script {
                    scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\" ,\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_First\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_First\" ,\"-s\", \"$Packetloss_Port\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_Second\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_Second\" ,\"-s\", \"$Packetloss_Port\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ANY\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"packet_loss\" ,\"infra_command_args\":{\"cli_args\":[\"packet_loss\" ,\"-l\" ,\"$Packetloss_Length_Third\" ,\"-h\" ,\"^api.gremlin.com\" ,\"-p\" ,\"^53\" ,\"-r\" ,\"$Packetloss_Percentage_Third\" ,\"-s\", \"$Packetloss_Port\"] ,\"providers\":[] ,\"type\":\"packet_loss\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",
                        returnStdout: true
                        ).trim()
                        echo "********************************************************************************************************"                        
                        echo "Scenario id for this $AttackType pipeline is ${scenario_id}"                        
                        echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                        echo "********************************************************************************************************"  
                }
            }
        }
        stage('Creating Chaos CPU Utilization Scenarios') {
            when {
                allOf{
                    expression { AttackType == 'CPU_Utilization' }
                    expression { Infrastructure == 'Kubernetes' }
                    }
               }
            agent any
            steps {
                script {
                    scenario_id = sh (
                        script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\" ,\"recommended_scenario_id\":\"\" ,\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_First\" ,\"-p\" ,\"$CPU_Util_Percentage_First\" ,\"-a\"] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\" ,\"next\":\"1\"} ,\"1\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"1\" ,\"next\":\"2\"} ,\"2\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_Second\" ,\"-p\" ,\"$CPU_Util_Percentage_Second\" ,\"-a\"] ,\"providers\":[] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"2\" ,\"next\":\"3\"} ,\"3\":{\"type\":\"Delay\" ,\"delay\":5 ,\"id\":\"3\" ,\"next\":\"4\"} ,\"4\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":100} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"cpu\" ,\"infra_command_args\":{\"cli_args\":[\"cpu\" ,\"-l\" ,\"$CPU_Util_Length_Third\" ,\"-p\" ,\"$CPU_Util_Percentage_Third\" ,\"-a\"] ,\"providers\":[] ,\"type\":\"cpu\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"4\"}} ,\"start_id\":\"0\"}}' --compressed",   
                        returnStdout: true
                        ).trim()
                        echo "********************************************************************************************************"                        
                        echo "Scenario id for this $AttackType pipeline is ${scenario_id}"                        
                        echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                        echo "********************************************************************************************************"  
                }
            }
        }        
        stage('Creating Chaos Shutdown Scenarios') {
            when {
                allOf{
                    expression { AttackType == 'Shutdown' }
                    expression { Infrastructure == 'Kubernetes' }
                    }
               }
            agent any
            steps {
                script {               
                        scenario_id = sh (
                            script: "curl -s 'https://api.gremlin.com/v1/scenarios?teamId=${GREMLIN_TEAM_ID}' -H 'Content-Type: application/json;charset=utf-8' -H 'Authorization: Key ${GREMLIN_API_KEY}' -d '{\"name\":\"$ClusterName-$AttackType-$DeploymentName\",\"recommended_scenario_id\":\"\",\"graph\":{\"nodes\":{\"0\":{\"target_definition\":{\"target_type\":\"Kubernetes\" ,\"strategy_type\":\"Random\" ,\"strategy\":{\"attrs\":{} ,\"type\":\"RandomPercent\" ,\"percentage\":\"100\"} ,\"k8s_objects\":[{\"cluster_id\":\"$ClusterName\" ,\"uid\":\"$DeploymentUID\" ,\"namespace\":\"$Namespace\" ,\"name\":\"$DeploymentName\" ,\"kind\":\"DEPLOYMENT\" ,\"labels\":{} ,\"annotations\":{} ,\"available_containers\":[] ,\"target_type\":\"Kubernetes\"}] ,\"containerSelection\":{\"selectionType\":\"ALL\" ,\"containerNames\":[]}} ,\"impact_definition\":{\"infra_command_type\":\"shutdown\" ,\"infra_command_args\":{\"cli_args\":[\"shutdown\" ,\"-d\" ,\"$Shutdown_Delay\" ,\"-r\"] ,\"type\":\"shutdown\"}} ,\"type\":\"InfraAttack\" ,\"id\":\"0\"}} ,\"start_id\":\"0\"}}' --compressed",    
                            returnStdout: true
                            ).trim()
                        echo "********************************************************************************************************"                        
                        echo "Scenario id for this $AttackType pipeline is ${scenario_id}"                        
                        echo "View scenario details at https://app.gremlin.com/scenarios/detail/${scenario_id}"
                    echo "********************************************************************************************************"
                }
            }
        }
    }
}
