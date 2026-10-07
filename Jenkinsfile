pipeline {
    agent any

    stages {
        stage('Validate Python') {
            steps {
                sh 'python3 --version'
                sh 'python3 -m py_compile delivery_metrics.py'
            }
        }

        stage('Validate Configuration') {
            steps {
                sh 'test -f prometheus.yml'
                sh 'test -f alert_rules.yml'
                sh 'test -f docker-compose.yml'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t delivery-monitoring:${BUILD_NUMBER} .'
            }
        }

        stage('Application Smoke Test') {
            steps {
                sh '''
                    docker rm -f delivery-metrics-test 2>/dev/null || true
                    docker run -d --name delivery-metrics-test -p 8000:8000 delivery-monitoring:${BUILD_NUMBER}
                    sleep 5
                    curl --fail http://localhost:8000/metrics
                '''
            }
        }
    }

    post {
        always {
            sh 'docker rm -f delivery-metrics-test 2>/dev/null || true'
            sh 'docker image rm delivery-monitoring:${BUILD_NUMBER} 2>/dev/null || true'
        }
    }
}
