pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'paycare-etl'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url:'https://github.com/JedhaBootcamp/paycare-jenkins-solution.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Run unit tests in Docker Container') {
            steps {
                script {
                    
                    // Run the Docker container with mounted input/output files
                    sh 'docker run --rm -v  "$(pwd):/app" ${DOCKER_IMAGE} pytest --junitxml=unit-tests.xml'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Create input data file dynamically
                    sh 'echo -e "employee_id,employee_name,salary\n 101,Alice,5000 \n 102,Bob,7000" > input_data.csv'

                    // Run the Docker container with mounted input/output files
                    sh 'docker run --rm -v "$(pwd):/app" ${DOCKER_IMAGE}'
                }
            }
        }
    }

    post {
        success {
            echo 'ETL Pipeline completed successfully!'
            // Optionally send notification (Slack/Email)
        }
        failure {
            echo 'ETL Pipeline failed.'
            // Optionally send notification (Slack/Email)
        }
    }
}