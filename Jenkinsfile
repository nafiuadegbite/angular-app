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

        stage('Deploy to Server') {
            steps {
                deployAppToServer()
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

def deployAppToServer() {
    script {
        sh 'nohup ng serve --prod &'
        def compilationStatus = waitForCompilation()
        
        if (compilationStatus == 'success') {
            sh 'pkill -f "ng serve"'
            echo 'Angular app compiled successfully and server stopped.'
        } else {
            echo 'Angular app compilation failed.'
        }
    }
}

def waitForCompilation() {
    def maxAttempts = 60
    def sleepDuration = 10
    
    for (int i = 0; i < maxAttempts; i++) {
        def compileOutput = sh(returnStdout: true, script: 'ng build --prod')
        if (compileOutput.contains('Complied successfully.')) {
            return 'success'
        }
        sleep sleepDuration
    }
    return 'failure'
}
