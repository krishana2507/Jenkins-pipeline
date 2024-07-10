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
        GIT_CREDENTIALS_ID = 'github-pat' // Use the ID you set in Jenkins for the PAT
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    git url: 'https://github.com/krishana2507/Jenkins-pipeline.git',
                        credentialsId: env.GIT_CREDENTIALS_ID,
                        branch: 'main'
                }
            }
        }
    }
}
