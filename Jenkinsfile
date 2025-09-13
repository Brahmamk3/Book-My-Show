pipeline {
    agent any

    tools {
        nodejs 'nodejs'          // Your Node.js installation name in Jenkins
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
                    sh ''' 
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=BMS \
                        -Dsonar.projectKey=BMS
                    '''
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
                subject: '$PROJECT_NAME Build #$BUILD_NUMBER - $BUILD_STATUS',
                body: """
                    <b>Project:</b> $PROJECT_NAME<br/>
                    <b>Build Number:</b> $BUILD_NUMBER<br/>
                    <b>Build URL:</b> <a href='$BUILD_URL'>$BUILD_URL</a><br/>
                    <b>Status:</b> $BUILD_STATUS
                """,
                mimeType: 'text/html',
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
            )
        }
    }
}
