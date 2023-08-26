pipeline {
    agent any

    environment {
        // Customize environment variables if needed
        NODE_HOME = tool 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    def npmHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    def npmBin = "${npmHome}/bin"
                    env.PATH = "${npmBin}:${env.PATH}"
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'ng build'  // Or 'ng build' depending on your setup
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
            }
        }

        // stage('Deploy to Server') {
        //     steps {
        //         script {
        //             sh 'ng serve'  // Or 'ng serve' depending on your setup
        //         }
        //     }
        // }
    }

    post {
        always {
            cleanWs()  // Clean up workspace after the build
        }
    }
}
