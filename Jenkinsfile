pipeline {
    agent any
    
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL of application source code.')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch of application source code.')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration yaml.')
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the repository using the provided GIT URL and branch
                    git url: params.Source_Code_GIT_URL, branch: params.Source_Code_GIT_Branch
                    // List files in the workspace to ensure checkout
                    sh 'ls -la'
                }
            }
        }
        
        stage('Read Config and Print YAML') {
            steps {
                script {
                    // Read the config.yaml file
                    def config = readYaml(file: "${params.Configuration_Yaml_Path}")

                    // Ensure oas_file_path exists in the config.yaml
                    if (config.oas_file_path) {
                        def oasFilePath = "${config.oas_file_path}"
                        
                        // Read and print the content of petstore.yaml
                        def yamlContent = readFile(file: oasFilePath)
                        echo "Contents of ${oasFilePath}:"
                        echo yamlContent
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

