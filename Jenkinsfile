pipeline {
    agent any

    tools {
        nodejs 'nodejs'          // Node.js installation in Jenkins
    }

    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/Brahmamk3/Book-My-Show.git', branch: 'main'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-tool') {
                    script {
                        def scannerHome = tool 'sonar-tool'
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=BMS \
                            -Dsonar.projectName=BMS \
                            -Dsonar.sources=.
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate abortPipeline: false, credentialsId: 'mysonar'
                    if (qg.status != 'OK') {
                        echo "Quality Gate status: ${qg.status}"
                    }
                }
            }
        }

        stage('Install Node.js & NPM') {
            steps {
                sh '''
                    curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
                    apt-get install -y nodejs
                    npm install
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t image1 .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub']) {
                    sh '''
                        docker tag image1 brahmamk015/bookmyshow:bookmyshowimage
                        docker push brahmamk015/bookmyshow:bookmyshowimage
                    '''
                }
            }
        }
    }

    post {
        always {
            emailext(
                to: 'sakamuriveera@gmail.com',
                subject: "${env.JOB_NAME} Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """
                    <b>Project:</b> ${env.JOB_NAME}<br/>
                    <b>Build Number:</b> ${env.BUILD_NUMBER}<br/>
                    <b>Build URL:</b> <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a><br/>
                    <b>Status:</b> ${currentBuild.currentResult}
                """,
                mimeType: 'text/html',
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
            )
        }
    }
}
