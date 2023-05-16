pipeline {
    agent any

    environment {
        GIT_URL = 'https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git'
        GIT_BRANCH = 'main'
        HELM_PATH = 'C:\\Users\\dkane\\Downloads\\helm-v3.12.0-windows-amd64\\windows-amd64\\helm.exe'
        KUBECONFIG_PATH = 'C:\\ProgramData\\Jenkins\\.kube\\config'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "refs/heads/${GIT_BRANCH}"]],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: "${GIT_URL}"]]
                ])
            }
        }

        stage('Create Namespace') {
            steps {
                script {
                    try {
                        bat "kubectl create namespace wp --kubeconfig=\"${env.KUBECONFIG_PATH}\""
                    } catch (Exception e) {
                        echo "Namespace 'wp' already exists, skipping..."
                    }
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    bat "\"${env.HELM_PATH}\" repo add bitnami https://charts.bitnami.com/bitnami --kubeconfig=\"${env.KUBECONFIG_PATH}\""
                    bat "\"${env.HELM_PATH}\" dependency build . --kubeconfig=\"${env.KUBECONFIG_PATH}\""
                    bat "\"${env.HELM_PATH}\" install final-project-wp-scalefocus bitnami/wordpress -n wp --kubeconfig=\"${env.KUBECONFIG_PATH}\" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce"
                }
            }
        }

        stage('Port Forwarding') {
            steps {
                script {
                    try {
                        bat "kubectl port-forward svc/final-project-wp-scalefocus 8080:80 --namespace=wp --kubeconfig=\"${env.KUBECONFIG_PATH}\""
                    } catch (Exception e) {
                        error("Failed to start port forwarding.")
                    }
                }
            }
        }
    }
}
