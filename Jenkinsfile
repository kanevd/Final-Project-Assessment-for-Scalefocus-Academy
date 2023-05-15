pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/kanevd/Final-Project-Assessment-for-Scalefocus-Academy.git'
            }
        }

        stage('Create Namespace') {
            steps {
                script {
                    try {
                        bat 'kubectl create namespace wp --kubeconfig="C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\WP-namespace\\kubeconfig.yaml"'
                        echo "Namespace 'wp' created."
                    } catch (Exception e) {
                        echo "Error creating the 'wp' namespace: ${e.getMessage()}"
                    }
                }
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    try {
                        bat 'helm install final-project-wp-scalefocus stable/wordpress -n wp --kubeconfig="C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\WP-namespace\\kubeconfig.yaml" --set wordpressUsername=admin,wordpressPassword=password,wordpressEmail=admin@example.com,persistence.enabled=true,persistence.storageClass=standard,persistence.accessMode=ReadWriteOnce'
                        echo "WordPress deployed."
                    } catch (Exception e) {
                        echo "Error deploying WordPress: ${e.getMessage()}"
                    }
                }
            }
        }
    }
}
