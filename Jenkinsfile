pipeline {
    agent any

    environment {
        // Initialize variables
        GIT_USERNAME = 'SahadevDahit'  // Update with your GitHub username
        GIT_TOKEN_CREDENTIALS_ID = 'access-token'  // Update with the ID of your GitHub PAT credential
    }

    parameters {
        string(name: 'DOCKERTAG', defaultValue: '', description: 'Docker tag for the image')
    }

    stages {
        stage('Clone repository') {
            steps {
                // Specify the repository URL and branch explicitly
                git branch: 'main', url: 'https://github.com/lnahuel1991/argocd-deployment.git'
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    // Retrieve Jenkins credentials for GitHub using PAT
                    withCredentials([string(credentialsId: GIT_TOKEN_CREDENTIALS_ID, variable: 'GIT_TOKEN')]) {
                        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                            sh "git config user.email lautaro.m.nahuel@gmail.com"
                            sh "git config user.name lnahuel1991"
                            sh "cat deployment.yml"
                            sh "sed -i 's+lnahuel/node-argocd.*+lnahuel/node-argocd:${params.DOCKERTAG}+g' deployment.yml"
                            sh "cat deployment.yml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${params.DOCKERTAG}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_TOKEN}@github.com/${GIT_USERNAME}/argocd-deployment.git HEAD:main"
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline execution successful!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
