pipeline {
    agent any
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
        string(name: 'Konnect_Token', description: 'Kong Konnect token')
    }
    environment {
        GIT_USER_EMAIL = 'krishna.sharma@neosalpha.com'
        GIT_USER_NAME = 'krishna2507'
    }
    stages {
        stage('Checkout Repository') {
            steps {
                script {
                    git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
                }
            }
        }
        stage('Read and Print YAML') {
            steps {
                script {
                    def configYamlPath = params.Configuration_Yaml_Path
                    def configContent = readFile(configYamlPath).trim()
                    echo "Config YAML Content:\n${configContent}"
                    
                    def config = readYaml text: configContent
                    if (config.oas_file_path) {
                        def oasFilePath = config.oas_file_path.trim()
                        echo "oas_file_path found: ${oasFilePath}"
                        
                        // Read and print the content of the OAS file
                        def oasContent = readFile(oasFilePath).trim()
                        echo "Contents of ${oasFilePath}:"
                        echo oasContent
                        
                        // Generate Kong config from OAS
                        sh "deck file openapi2kong -s ${oasFilePath} -o kong.yaml"
                        
                        // Read and print the content of the generated kong.yaml file
                        def kongConfigContent = readFile('kong.yaml').trim()
                        echo "Generated Kong config (kong.yaml) Content:\n${kongConfigContent}"
                        
                        // Append all plugin configurations specified in plugin_file_path using yq
                        if (config.plugin_file_path) {
                            config.plugin_file_path.each { pluginFilePath ->
                                pluginFilePath = pluginFilePath.trim()
                                echo "Appending plugin configuration from: ${pluginFilePath}"
                                
                                // Read the plugin configuration
                                def pluginConfig = readYaml(file: pluginFilePath)
                                
                                // Load kong.yaml
                                def kongConfig = readYaml(file: 'kong.yaml')
                                
                                // Ensure the services array exists
                                if (kongConfig.services == null) {
                                    error "No services defined in kong.yaml"
                                }
                                
                                // Add plugins to each service
                                kongConfig.services.each { service ->
                                    if (service.plugins == null) {
                                        service.plugins = []
                                    }
                                    service.plugins.add(pluginConfig)
                                }
                                
                                // Write back the updated kong.yaml
                                writeYaml(file: 'kong.yaml', data: kongConfig)
                            }
                            
                            // Remove _format_version using sed
                            sh "sed -i '/_format_version: \"3.0\"/d' kong.yaml"
                            
                            // Print updated kong.yaml content
                            def updatedKongConfigContent = readFile('kong.yaml').trim()
                            echo "Updated Kong config (kong.yaml) Content:\n${updatedKongConfigContent}"
                        } else {
                            error "plugin_file_path not found in ${params.Configuration_Yaml_Path}"
                        }
                    } else {
                        error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
                    }
                }
            }
        }
        stage('Push Kong YAML to Kong Konnect') {
            steps {
                script {
                    def konnectToken = params.Konnect_Token
                    def konnectControlPlaneName = 'konnect-values'
                    def deckCmd = "deck sync -s kong.yaml --konnect-token=${konnectToken} --konnect-control-plane-name=${konnectControlPlaneName}"
                    
                    def result = sh(script: deckCmd, returnStatus: true)
                    
                    if (result == 0) {
                        echo "Successfully pushed kong.yaml to Kong Konnect"
                    } else {
                        error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
                    }
                }
            }
        }
    }
}





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
//                             // Ensure the plugins section exists and is an array
//                             sh "yq eval '.plugins = ( .plugins // [] )' -i kong.yaml"
                            
//                             config.plugin_file_path.each { pluginFilePath ->
//                                 pluginFilePath = pluginFilePath.trim()
//                                 echo "Appending plugin configuration from: ${pluginFilePath}"
                                
//                                 // Append the plugin configuration
//                                 sh "yq eval-all '.plugins += load(\"${pluginFilePath}\")' -i kong.yaml"
//                             }
                            
//                             // Remove _format_version using sed
//                             sh "sed -i '/_format_version: \"3.0\"/d' kong.yaml"
                            
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

