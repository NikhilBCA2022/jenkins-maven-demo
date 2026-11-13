pipeline {
    agent any

    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run the Test stage?')
        choice(name: 'BUILD_GOAL', choices: ['package', 'verify'], description: 'Maven goal for the Build stage')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo "Building with goal: ${params.BUILD_GOAL}"
                sh "mvn -B -DskipTests clean ${params.BUILD_GOAL}"
            }
        }
        stage('Test') {
            when {
                expression { params.RUN_TESTS }
            }
            steps {
                sh 'mvn -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
