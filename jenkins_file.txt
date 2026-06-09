cat > Jenkinsfile <<'EOF'
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t poorvitha-devops-demo .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f poorvitha-demo || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d --name poorvitha-demo -p 8082:80 poorvitha-devops-demo'
            }
        }

        stage('Verify App') {
            steps {
                sh 'curl localhost:8082'
            }
        }
    }
}
EOF