pipeline {
    agent any
    parameters {
        string(name: 'Konnect_Token', description: 'Kong Konnect token')
    }
    environment {
        GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
        GIT_USER_NAME = 'krishna2507'
    }
    stages {
        stage('Read API Spec Details from CSV') {
            steps {
                script {
                    // Define the path to the CSV file
                    def csvFilePath = 'kong.csv'
                    
                    // Read the CSV content
                    def csvContent = readFile(csvFilePath).trim()
                    
                    // Print the CSV content
                    echo "CSV Content:\n${csvContent}"

                    // Split CSV into lines (rows)
                    def csvLines = csvContent.split("\n")

                    // Ensure headers are split correctly into an array of strings
                    def headers = csvLines[0].split(",").collect { it.trim() }

                    // Extract the first row of data (after headers)
                    def values = csvLines[1].split(",").collect { it.trim() }

                    // Assuming the first header corresponds to a file path
                    def firstHeader = headers[0] // This will give you the name of the first column
                    def filePath = values[0] // This will give you the value from the first column of the first row

                    // Print the file path and its content
                    echo "File Path from First Header (${firstHeader}): ${filePath}"

                    // Print the content of the specified file
                    if (fileExists(filePath)) {
                        def fileContent = readFile(filePath).trim()
                        echo "Content of ${filePath}:\n${fileContent}"
                    } else {
                        echo "File not found at path: ${filePath}"
                    }
                }
            }
        }
    }
}






// pipeline {
//     agent any
//     parameters {
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Read API Spec Details from CSV') {
//             steps {
//                 script {
//                     // Define the path to the CSV file
//                     def csvFilePath = 'kong.csv'
                    
//                     // Read the CSV content
//                     def csvContent = readFile(csvFilePath).trim()
                    
//                     // Print the CSV content
//                     echo "CSV Content:\n${csvContent}"
//                 }
//             }
//         }
//     }
// }










































// pipeline {
//     agent any
//     parameters {
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Read API Spec Details from CSV') {
//             steps {
//                 script {
//                     // Define the path to the CSV file
//                     def csvFilePath = 'kong.csv'
                    
//                     // Read the CSV content
//                     def csvContent = readFile(csvFilePath).trim()
                    
//                     // Split CSV into lines (rows)
//                     def csvLines = csvContent.split("\n")
                    
//                     // Ensure headers are split correctly into an array of strings
//                     def headers = csvLines[0].split(",").collect { it.trim() }
                    
//                     // Extract the first row of data (after headers)
//                     def values = csvLines[1].split(",").collect { it.trim() }
                    
//                     // Find indices of specific columns based on headers
//                     def apiNameIndex = headers.indexOf('API Name')
//                     def specUrlIndex = headers.indexOf('Spec URL')
//                     def pluginIndex = headers.indexOf('Plugin')
//                     def limitIndex = headers.indexOf('limit')
//                     def windowSizeIndex = headers.indexOf('window size')
                    
//                     // Extract the values using the indices
//                     def apiName = values[apiNameIndex]
//                     def specUrl = values[specUrlIndex]
//                     def plugin = values[pluginIndex]
//                     def limit = values[limitIndex]
//                     def windowSize = values[windowSizeIndex]
                    
//                     echo "API Name: ${apiName}"
//                     echo "Spec URL: ${specUrl}"
//                     echo "Plugin: ${plugin}"
//                     echo "Limit: ${limit}"
//                     echo "Window Size: ${windowSize}"
                    
//                     // Checkout the spec repository
//                     dir('spec_repo') {
//                         git url: specUrl, branch: 'main'  // Assuming 'main' branch
//                     }
                    
//                     // OAS file path is assumed inside the cloned repo
//                     def oasFilePath = "spec_repo/petstore.yaml"
                    
//                     // Generate Kong config from OAS
//                     sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                    
//                     // Apply the plugin settings
//                     sh """
//                     yq eval '.plugins += [{
//                         "name": "${plugin}",
//                         "config": {
//                             "limit": ${limit},
//                             "window_size": ${windowSize}
//                         }
//                     }]' -i kong.yaml
//                     """
                    
//                     // Print the final kong.yaml
//                     def kongConfigContent = readFile('kong.yaml').trim()
//                     echo "Updated Kong config (kong.yaml) Content:\n${kongConfigContent}"
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }







// Correct code for the kong artifacts
// pipeline {  
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps { 
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_repo) {
//                         def oasRepoUrl = config.oas_repo.url.trim()
//                         def oasRepoBranch = config.oas_repo.branch.trim()
//                         def oasFilePath = config.oas_repo.file_path.trim()
                        
//                         // Checkout the OAS repo
//                         dir('oas-repo') {
//                             git url: oasRepoUrl, branch: oasRepoBranch
//                         }
                        
//                         def fullOasFilePath = "oas-repo/${oasFilePath}"
//                         echo "oas_file_path found in different repo: ${fullOasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(fullOasFilePath).trim()
//                         echo "Contents of ${fullOasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${fullOasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
//                         // Append global plugin configurations
//                         if (config.global_file_path) {
//                             config.global_file_path.each { globalFilePath ->
//                                 globalFilePath = globalFilePath.trim()
//                                 echo "Processing global plugin configuration from: ${globalFilePath}"
                                
//                                 // Remove specific lines from the plugin configuration file
//                                 sh "sed -i '/_format_version: \"3.0\"/d' ${globalFilePath}"
//                                 sh "sed -i '/^plugins:/d' ${globalFilePath}"
                                
//                                 // Append the global plugin configuration
//                                 sh "yq eval-all '.plugins += load(\"${globalFilePath}\")' -i kong.yaml"
//                             }
                            
//                             // Print updated kong.yaml content with global plugins
//                             def updatedKongConfigContent = readFile('kong.yaml').trim()
//                             echo "Updated Kong config (kong.yaml) with global plugins:\n${updatedKongConfigContent}"
//                         }
                        
//                         // Append service-specific plugin configurations
//                         if (config.plugin_file_path) {
//                             config.plugin_file_path.each { pluginFilePath ->
//                                 pluginFilePath = pluginFilePath.trim()
//                                 echo "Processing service-specific plugin configuration from: ${pluginFilePath}"
                                
//                                 // Remove specific lines from the plugin configuration file
//                                 sh "sed -i '/_format_version: \"3.0\"/d' ${pluginFilePath}"
//                                 sh "sed -i '/^plugins:/d' ${pluginFilePath}"
                                
//                                 // Append the plugin configuration to the specified service
//                                 sh "yq eval-all '.services[] |= (select(.name == \"swagger-petstore-openapi-3-0\") | .plugins += load(\"${pluginFilePath}\") | .)' -i kong.yaml"
//                             }
                            
//                             // Print updated kong.yaml content with service-specific plugins
//                             def updatedKongConfigContent = readFile('kong.yaml').trim()
//                             echo "Updated Kong config (kong.yaml) with service-specific plugins:\n${updatedKongConfigContent}"
//                         } else {
//                             error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
//                         }
//                     } else {
//                         error "oas_repo not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }







// Setup of CP and DP on kong gateway using script
// pipeline {     
//     agent any 

//     environment {
//         AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
//         AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
//         AWS_REGION = 'ap-south-1'
//         CLUSTER_NAME = 'kong-EKSClusterRole'
//         HELM_BIN = '/usr/local/bin/helm'
//         SERVICE_ACCOUNT = 'jenkins-sa'
//         NAMESPACE_CP = 'kong-cp'
//         NAMESPACE_DP = 'kong-dp'
//         CERTS_DIR = './certs' // Path to your certificate files
//         LICENSE_FILE = 'license.json' // Path to your Kong license file
//     }

//     stages {
//         stage('Configure AWS CLI') {
//             steps {
//                 withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
//                                  string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
//                     sh '''
//                     aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
//                     aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
//                     aws configure set region $AWS_REGION
//                     '''
//                 }
//             }
//         }

//         stage('Update kubeconfig') {
//             steps {
//                 sh '''
//                 aws eks update-kubeconfig --name $CLUSTER_NAME --region $AWS_REGION
//                 '''
//             }
//         }

//         stage('Setup Namespaces') {
//             steps {
//                 script {
//                     sh 'kubectl create namespace $NAMESPACE_CP || true'
//                     sh 'kubectl create namespace $NAMESPACE_DP || true'
//                 }
//             }
//         }

//         stage('Create Secrets') {
//             steps {
//                 script {
//                     sh """
//                         kubectl create secret tls kong-cluster-cert --cert=${CERTS_DIR}/tls.crt --key=${CERTS_DIR}/tls.key -n $NAMESPACE_CP || true
//                         kubectl create secret tls kong-cluster-cert --cert=${CERTS_DIR}/tls.crt --key=${CERTS_DIR}/tls.key -n $NAMESPACE_DP || true
                        
//                         kubectl create secret generic postgresql-password --from-literal=postgres-password=kong --from-literal=password=kong -n $NAMESPACE_CP || true
                        
//                         kubectl create secret generic kong-enterprise-license --from-file=license=${LICENSE_FILE} -n $NAMESPACE_CP || true
//                         kubectl create secret generic kong-enterprise-license --from-file=license=${LICENSE_FILE} -n $NAMESPACE_DP || true
                        
//                         kubectl create secret generic kong-enterprise-superuser-password --from-literal=password=password -n $NAMESPACE_CP || true
                        
//                         x='{"cookie_name":"admin_session","storage":"kong","cookie_samesite":"off","cookie_secure":false,"secret":"secret"}'
//                         kubectl create secret generic kong-session-config --from-literal=admin_gui_session_conf="${x}" -n $NAMESPACE_CP || true
//                     """
//                 }
//             }
//         }

//         stage('Deploy Control Plane') {
//             steps {
//                 script {
//                     sh """
//                         helm repo add kong https://charts.konghq.com
//                         helm repo update
//                         helm install -f ./kong_values/kong_cp_values.yaml kong-cp kong/kong --namespace $NAMESPACE_CP
//                     """
//                 }
//             }
//         }

//         stage('Deploy Data Plane') {
//             steps {
//                 script {
//                     def cluster_ep = "kong-cp-kong-cluster.$NAMESPACE_CP.svc.cluster.local:8005"
//                     def telem_ep = "kong-cp-kong-clustertelemetry.$NAMESPACE_CP.svc.cluster.local:8006"
                    
//                     sh """
//                         helm install -f ./deployment_yamls/kong_dp_values.yaml kong-dp --set env.cluster_control_plane=${cluster_ep} --set env.cluster_telemetry_endpoint=${telem_ep} kong/kong --namespace $NAMESPACE_DP
//                     """
//                 }
//             }
//         }
//     }    
// }










// Setup of DP on konnect script
// pipeline {     
//     agent any 

//     environment {
//         AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
//         AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
//         AWS_REGION = 'ap-south-1'
//         CLUSTER_NAME = 'kong-EKSClusterRole'
//         HELM_BIN = '/usr/local/bin/helm'
//         SERVICE_ACCOUNT = 'jenkins-sa'
//         NAMESPACE = 'kong-konnect'
//     }

//     stages {
//         stage('Configure AWS CLI') {
//             steps {
//                 withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
//                                  string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
//                     sh '''
//                     aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
//                     aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
//                     aws configure set region $AWS_REGION
//                     '''
//                 }
//             }
//         }

//         stage('Update kubeconfig') {
//             steps {
//                 sh '''
//                 aws eks update-kubeconfig --name $CLUSTER_NAME --region $AWS_REGION
//                 '''
//             }
//         }

//         stage('Create Namespace') {
//             steps {
//                 sh '''
//                 kubectl create namespace kong-konnect || echo "Namespace already exists"
//                 '''
//             }
//         }

//         stage('Add Helm Repo') {
//             steps {
//                 sh '''
//                 ${HELM_BIN} repo add kong https://charts.konghq.com
//                 ${HELM_BIN} repo update
//                 '''
//             }
//         }

//         stage('Create Secret') {
//             steps {
//                 sh '''
//                 kubectl create secret tls kong-cluster-cert -n kong-konnect --cert=certs/tls.crt --key=certs/tls.key
//                 '''
//             }
//         }

//         stage('Install Kong Gateway') {
//             steps {
//                 sh '''
//                 ${HELM_BIN} install my-kong kong/kong -n kong-konnect --values values/values.yaml
//                 '''
//             }
//         }

//         stage('Verify Kong Installation') {
//             steps {
//                 sh '''
//                 kubectl get pods --namespace kong-konnect
//                 kubectl get services --namespace kong-konnect
//                 '''
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Kong Gateway installed successfully on EKS!'
//         }
//         failure {
//             echo 'Failed to install Kong Gateway on EKS.'
//         }
//     }
// }



// pipeline {  
//     agent any

//     environment {
//         AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
//         AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
//         AWS_REGION = 'ap-south-1'
//         CLUSTER_NAME = 'kong-EKSClusterRole'
//         HELM_BIN = '/usr/local/bin/helm'
//     }

//     stages {
//         stage('Configure AWS CLI') {
//             steps {
//                 sh '''
//                 aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
//                 aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
//                 aws configure set region $AWS_REGION
//                 '''
//             }
//         }

//         stage('Update kubeconfig') {
//             steps {
//                 sh '''
//                 aws eks update-kubeconfig --name $CLUSTER_NAME --region $AWS_REGION
//                 '''
//             }
//         }

//         stage('Create Namespace') {
//             steps {
//                 sh '''
//                 kubectl create namespace kong || echo "Namespace already exists"
//                 '''
//             }
//         }

//         stage('Add Helm Repo') {
//             steps {
//                 sh '''
//                 ${HELM_BIN} repo add kong https://charts.konghq.com
//                 ${HELM_BIN} repo update
//                 '''
//             }
//         }

//         stage('Create Secret') {
//             steps {
//                 sh '''
//                 kubectl create secret tls kong-cluster-cert -n kong --cert=certs/tls.crt --key=certs/tls.key
//                 '''
//             }
//         }

//         stage('Install Kong Gateway') {
//             steps {
//                 sh '''
//                 ${HELM_BIN} install my-kong kong/kong -n kong --values values/values.yaml
//                 '''
//             }
//         }

//         stage('Verify Kong Installation') {
//             steps {
//                 sh '''
//                 kubectl get pods --namespace kong
//                 kubectl get services --namespace kong
//                 '''
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Kong Gateway installed successfully on EKS!'
//         }
//         failure {
//             echo 'Failed to install Kong Gateway on EKS.'
//         }
//     }
// }




// correct code 
// pipeline {  
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps { 
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(oasFilePath).trim()
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
//                         // Append global plugin configurations
//                         if (config.global_file_path) {
//                             config.global_file_path.each { globalFilePath ->
//                                 globalFilePath = globalFilePath.trim()
//                                 echo "Processing global plugin configuration from: ${globalFilePath}"
                                
//                                 // Remove specific lines from the plugin configuration file
//                                 sh "sed -i '/_format_version: \"3.0\"/d' ${globalFilePath}"
//                                 sh "sed -i '/^plugins:/d' ${globalFilePath}"
                                
//                                 // Append the global plugin configuration
//                                 sh "yq eval-all '.plugins += load(\"${globalFilePath}\")' -i kong.yaml"
//                             }
                            
//                             // Print updated kong.yaml content with global plugins
//                             def updatedKongConfigContent = readFile('kong.yaml').trim()
//                             echo "Updated Kong config (kong.yaml) with global plugins:\n${updatedKongConfigContent}"
//                         }
                        
//                         // Append service-specific plugin configurations
//                         if (config.plugin_file_path) {
//                             config.plugin_file_path.each { pluginFilePath ->
//                                 pluginFilePath = pluginFilePath.trim()
//                                 echo "Processing service-specific plugin configuration from: ${pluginFilePath}"
                                
//                                 // Remove specific lines from the plugin configuration file
//                                 sh "sed -i '/_format_version: \"3.0\"/d' ${pluginFilePath}"
//                                 sh "sed -i '/^plugins:/d' ${pluginFilePath}"
                                
//                                 // Append the plugin configuration to the specified service
//                                 sh "yq eval-all '.services[] |= (select(.name == \"swagger-petstore-openapi-3-0\") | .plugins += load(\"${pluginFilePath}\") | .)' -i kong.yaml"
//                             }
                            
//                             // Print updated kong.yaml content with service-specific plugins
//                             def updatedKongConfigContent = readFile('kong.yaml').trim()
//                             echo "Updated Kong config (kong.yaml) with service-specific plugins:\n${updatedKongConfigContent}"
//                         } else {
//                             error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
//                         }
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }





// pipeline { 
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(oasFilePath).trim()
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
//                         // Append all plugin configurations specified in plugin_file_path using yq
//                         if (config.plugin_file_path) {
//                              // Remove _format_version using sed
//                             sh "sed -i '/_format_version: \"3.0\"/d' kong.yaml"
//                             // Ensure the plugins section exists and is an array
//                             sh "yq eval '.plugins = ( .plugins // [] )' -i kong.yaml"
                            
//                             config.plugin_file_path.each { pluginFilePath ->
//                                 pluginFilePath = pluginFilePath.trim()
//                                 echo "Appending plugin configuration from: ${pluginFilePath}"
                                
//                                 // Append the plugin configuration
//                                 sh "yq eval-all '.plugins += load(\"${pluginFilePath}\")' -i kong.yaml"
//                             }
                            
//                             // Print updated kong.yaml content
//                             def updatedKongConfigContent = readFile('kong.yaml').trim()
//                             echo "Updated Kong config (kong.yaml) Content:\n${updatedKongConfigContent}"
//                         } else {
//                             error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
//                         }
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }





// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(oasFilePath).trim()
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
//                         // Append key-auth plugin configuration using yq
//                         if (config.plugin_file_path) {
//                             def keyAuthPluginPath = config.plugin_file_path.find { it.contains("keyauth.yaml") }
//                             if (keyAuthPluginPath) {
//                                 keyAuthPluginPath = keyAuthPluginPath.trim()
//                                 echo "Appending plugin configuration from: ${keyAuthPluginPath}"
                                
//                                 // Ensure the plugins section exists and is an array
//                                 sh "yq eval '.plugins = []' -i kong.yaml"
//                                 sh "yq eval '.plugins += load(\"${keyAuthPluginPath}\")' -i kong.yaml"
                                
//                                 // Remove _format_version using sed
//                                 sh "sed -i '/_format_version: \"3.0\"/d' kong.yaml"
                                
//                                 // Print updated kong.yaml content
//                                 def updatedKongConfigContent = readFile('kong.yaml').trim()
//                                 echo "Updated Kong config (kong.yaml) Content:\n${updatedKongConfigContent}"
//                             } else {
//                                 error "keyauth.yaml not found in plugin_file_path"
//                             }
//                         } else {
//                             error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
//                         }
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }




// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(oasFilePath).trim()
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
//                         // Append key-auth plugin configuration using yq
//                         if (config.plugin_file_path) {
//                             def keyAuthPluginPath = config.plugin_file_path.find { it.contains("keyauth.yaml") }
//                             if (keyAuthPluginPath) {
//                                 keyAuthPluginPath = keyAuthPluginPath.trim()
//                                 echo "Appending plugin configuration from: ${keyAuthPluginPath}"
//                                 sh "yq eval '.plugins += load(\"${keyAuthPluginPath}\")' -i kong.yaml"
                                
//                                 // Print updated kong.yaml content
//                                 def updatedKongConfigContent = readFile('kong.yaml').trim()
//                                 echo "Updated Kong config (kong.yaml) Content:\n${updatedKongConfigContent}"
//                             } else {
//                                 error "keyauth.yaml not found in plugin_file_path"
//                             }
//                         } else {
//                             error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
//                         }
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }






// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//         string(name: 'Konnect_Token', description: 'Kong Konnect token')
//     }
//     environment {
//         GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
//         GIT_USER_NAME = 'krishna2507'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile(configYamlPath).trim()
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile(oasFilePath).trim()
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
                        
//                         // Generate Kong config from OAS
//                         sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
//                         // Read and print the content of the generated kong.yaml file
//                         def kongConfigContent = readFile('kong.yaml').trim()
//                         echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//         stage('Push Kong YAML to Kong Konnect') {
//             steps {
//                 script {
//                     def konnectToken = params.Konnect_Token
//                     def konnectControlPlaneName = 'konnect-values'
//                     def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
//                     def result = sh(script: deckCmd, returnStatus: true)
                    
//                     if (result == 0) {
//                         echo "Successfully pushed kong.yaml to Kong Konnect"
//                     } else {
//                         error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
//                     }
//                 }
//             }
//         }
//     }
// }







// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//     }
//     environment {
//         ADMIN_API_URL = 'https://us.api.konghq.com/v2'
//         ADMIN_API_TOKEN = 'your_admin_api_token'
//         CONTROL_PLANE_NAME = 'your_control_plane_name'
//         GATEWAY_SERVICE_NAME = 'your_gateway_service_name'
//         API_PRODUCT_ID = 'your_api_product_id'
//         API_PRODUCT_VERSION_ID = 'your_api_product_version_id' 
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def configYamlPath = params.Configuration_Yaml_Path
//                     def configContent = readFile configYamlPath
//                     echo "Config YAML Content:\n${configContent}"
                    
//                     def config = readYaml text: configContent
//                     if (config.oas_file_path) {
//                         def oasFilePath = config.oas_file_path.trim()
//                         echo "oas_file_path found: ${oasFilePath}"
                        
//                         // Read and print the content of the OAS file
//                         def oasContent = readFile oasFilePath
//                         echo "Contents of ${oasFilePath}:"
//                         echo oasContent
//                     } else {
//                         error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
//                     }
//                 }
//             }
//         }
//     }
// }





// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//     }
//     environment {
//         ADMIN_API_URL = 'https://us.api.konghq.com/v2'
//         ADMIN_API_TOKEN = 'your_admin_api_token'
//         CONTROL_PLANE_NAME = 'your_control_plane_name'
//         GATEWAY_SERVICE_NAME = 'your_gateway_service_name'
//         API_PRODUCT_ID = 'your_api_product_id'
//         API_PRODUCT_VERSION_ID = 'your_api_product_version_id'
//     }
//     stages {
//         stage('Checkout Repository') {
//             steps {
//                 script {
//                     git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
//                 }
//             }
//         }
//         stage('Read and Print YAML') {
//             steps {
//                 script {
//                     def yamlFilePath = params.Configuration_Yaml_Path
//                     def yamlContent = readFile yamlFilePath
//                     echo "YAML Content:\n${yamlContent}"
//                 }
//             }
//         }
//     }
// }











// pipeline {
//     agent any
//     parameters {
//         string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
//         string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
//         string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
//     }
//     environment {
//         ADMIN_API_URL = 'https://us.api.konghq.com/v2'
//         ADMIN_API_TOKEN = 'your_admin_api_token'
//         CONTROL_PLANE_NAME = 'your_control_plane_name'
//         GATEWAY_SERVICE_NAME = 'your_gateway_service_name'
//         API_PRODUCT_ID = 'your_api_product_id'
//         API_PRODUCT_VERSION_ID = 'your_api_product_version_id'
//     }
//     stages {
//         stage('Validate Parameters') {
//             steps {
//                 script {
//                     echo "Source_Code_GIT_URL: ${params.Source_Code_GIT_URL}"
//                     echo "Source_Code_GIT_Branch: ${params.Source_Code_GIT_Branch}"
//                     echo "Configuration_Yaml_Path: ${params.Configuration_Yaml_Path}"
//                 }
//             }
//         }
//     }
// }

