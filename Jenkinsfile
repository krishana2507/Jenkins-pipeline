pipeline {
    agent any
    parameters {
        string(name: 'API_REPO_NAME', description: 'API Repository name')
        string(name: 'API_REPO_BRANCH', description: 'Choose the branch name for API Repository')
        string(name: 'GLOBAL_CONFIG_REPO_NAME', description: 'Global Config Repository name')
        string(name: 'GLOBAL_CONFIG_BRANCH', description: 'Choose the branch name for Global Config Repository')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'prod', 'test'], description: 'The environment to deploy to')
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
                    echo "API_REPO_NAME: ${params.API_REPO_NAME}"
                    echo "API_REPO_BRANCH: ${params.API_REPO_BRANCH}"
                    echo "GLOBAL_CONFIG_REPO_NAME: ${params.GLOBAL_CONFIG_REPO_NAME}"
                    echo "GLOBAL_CONFIG_BRANCH: ${params.GLOBAL_CONFIG_BRANCH}"
                    echo "ENVIRONMENT: ${params.ENVIRONMENT}"
                }
            }
        }
    }
}
