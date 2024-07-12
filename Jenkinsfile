pipeline {
    agent any
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL of application source code.\ne.g. - https://alm-github.systems.uk.hsbc/myorg/myapplication.git')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch of application source code.\ne.g. - master eg: scenario-')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration yaml\ne.g. - config.yaml')
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
        stage('Read Config and Print OAS File') {
            steps {
                script {
                    try {
                        // Read the config.yaml file to get the path to the OAS file
                        def config = readYaml file: params.Configuration_Yaml_Path
                        echo "Config: ${config}"

                        def oasFilePath = config.oas_file_path
                        echo "OAS file path from config: ${oasFilePath}"

                        // Ensure the OAS file exists
                        if (fileExists(oasFilePath)) {
                            // Read and print the contents of the OAS file
                            def oasFileContent = readFile file: oasFilePath
                            echo "Contents of OAS file (${oasFilePath}):\n${oasFileContent}"
                        } else {
                            error "OAS file (${oasFilePath}) does not exist."
                        }
                    } catch (Exception e) {
                        echo "An error occurred: ${e.message}"
                        throw e
                    }
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

