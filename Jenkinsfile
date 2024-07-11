pipeline {
    agent any
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL of application source code.\ne.g. - https://alm-github.systems.uk.hsbc/myorg/myapplication.git')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch of application source code.\ne.g. - master eg: scenario-')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration yaml\ne.g. - kong/dev-config.yaml, src/main/resources/pipeline-config.yaml etc.')
        string(name: 'GSD_CR', description: 'Enter a valid GSD CR or IN that is approved by HAP.\nThis is mandatory for all deployment to Pre-Production and Production. CR3169492 IN0607525')
    }
    environment {
        ADMIN_API_URL = 'https://us.api.konghq.com/v2'
        ADMIN_API_TOKEN = 'your_admin_api_token'
        CONTROL_PLANE_NAME = 'your_control_plane_name'
        GATEWAY_SERVICE_NAME = 'your_gateway_service_name'
        API_PRODUCT_ID = 'your_api_product_id'
        API_PRODUCT_VERSION_ID = 'your_api_product_version_id'
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
                    def yamlFilePath = params.Configuration_Yaml_Path
                    def yamlContent = readFile yamlFilePath
                    echo "YAML Content:\n${yamlContent}"
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

