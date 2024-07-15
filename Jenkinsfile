pipeline {
    agent any
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
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
                        
                        // Push kong.yaml to Kong Konnect UI using Deck
                        stage('Push Kong YAML to Kong Konnect') {
                            steps {
                                script {
                                    def konnectToken = credentials('konnect-token') // Replace with your credential ID
                                    def konnectControlPlaneName = 'konnect-values'
                                    def deckCmd = "deck sync kong.yaml --konnect-token ${konnectToken} --konnect-control-plane-name ${konnectControlPlaneName}"
                                    
                                    def result = sh(script: deckCmd, returnStatus: true)
                                    
                                    if (result == 0) {
                                        echo "Successfully pushed kong.yaml to Kong Konnect"
                                    } else {
                                        error "Failed to push kong.yaml to Kong Konnect. Deck command returned non-zero exit code."
                                    }
                                }
                            }
                        }
                        
                        // Stage to commit files
                        stage('Commit files') {
                            steps {
                                script {
                                    git config --local user.email "krishna.sharma@neosalpha.com"
                                    git config --local user.name "krishna2507"
                                    git add '*.yaml'
                                    
                                    def hasChanges = sh(script: 'git status --porcelain', returnStdout: true).trim()
                                    
                                    if (hasChanges.empty) {
                                        echo "::set-output name=push::false"
                                    } else {
                                        git commit -m "Add changes sd" -a
                                        echo "::set-output name=push::true"
                                        
                                        if (env.BRANCH_NAME == 'main') {
                                            // Push changes only for the main branch
                                            gitPushChanges()
                                        } else {
                                            echo 'Skipping push changes as branch is not main.'
                                        }
                                    }
                                }
                            }
                        }
                        
                    } else {
                        error "oas_file_path not found in ${params.Configuration_Yaml_Path}"
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

