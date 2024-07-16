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
                git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
            }
        }
        stage('Read and Print YAML') {
            steps {
                script {
                    try {
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
                        } else {
                            error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
                        }
                    } catch (Exception e) {
                        error "Failed to read and process YAML: ${e.getMessage()}"
                    }
                }
            }
        }
        stage('Commit kong.yaml') {
            steps {
                script {
                    try {
                        sh "git config --local user.email '${GIT_USER_EMAIL}'"
                        sh "git config --local user.name '${GIT_USER_NAME}'"
                        sh "git add kong.yaml"
                        
                        def hasChanges = sh(script: 'git status --porcelain', returnStdout: true).trim()
                        
                        if (!hasChanges.isEmpty()) {
                            sh 'git commit -m "Add generated kong.yaml"'
                        } else {
                            echo "No changes to commit."
                        }
                    } catch (Exception e) {
                        error "Failed to commit kong.yaml: ${e.getMessage()}"
                    }
                }
            }
        }
        stage('Push kong.yaml to Repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    script {
                        try {
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${params.Source_Code_GIT_URL} ${params.Source_Code_GIT_Branch}"
                            echo "Committed and pushed kong.yaml to the repository."
                        } catch (Exception e) {
                            error "Failed to push kong.yaml: ${e.getMessage()}"
                        }
                    }
                }
            }
        }
        stage('Push Kong YAML to Kong Konnect') {
            steps {
                script {
                    try {
                        def konnectToken = params.Konnect_Token
                        def konnectControlPlaneName = 'konnect-values'
                        def deckCmd = "deck sync kong.yaml --konnect-token ${konnectToken} --konnect-control-plane-name ${konnectControlPlaneName}"
                        
                        def result = sh(script: deckCmd, returnStatus: true)
                        
                        if (result == 0) {
                            echo "Successfully pushed kong.yaml to Kong Konnect"
                        } else {
                            error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
                        }
                    } catch (Exception e) {
                        error "Failed to push kong.yaml to Kong Konnect: ${e.getMessage()}"
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

