pipeline {
    agent any

    environment {
        GIT_HOME = tool 'Git'
        NODE_HOME = tool 'NodeJS'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/nafiuadegbite/angular-app.git']]])
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    def npmHome = tool(name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation')
                    env.PATH = "${npmHome}/bin:${env.PATH}"
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'ng build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
}

