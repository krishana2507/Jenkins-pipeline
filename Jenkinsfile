pipeline {
    agent any
    parameters {
        string(name: 'Source_Code_GIT_URL', description: 'Enter GIT URL')
        string(name: 'Source_Code_GIT_Branch', description: 'Enter GIT branch')
        string(name: 'Configuration_Yaml_Path', description: 'File path for configuration')
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
        stage('Validate Parameters') {
            steps {
                script {
                    echo "Source_Code_GIT_URL: ${params.Source_Code_GIT_URL}"
                    echo "Source_Code_GIT_Branch: ${params.Source_Code_GIT_Branch}"
                    echo "Configuration_Yaml_Path: ${params.Configuration_Yaml_Path}"
                }
            }
        }
    }
}

