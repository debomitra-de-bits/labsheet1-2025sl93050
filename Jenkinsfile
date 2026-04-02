pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Build stage (no compilation needed for Python)'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                python3 -c "
from calculator import add, multiply, subtract, divide

assert add(2,3) == 5
assert multiply(2,3) == 6
assert subtract(5,2) == 3
assert divide(10,2) == 5

print('All tests passed')
"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to EC2...'
                sshagent(['aws-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no calculator.py ec2-user@51.21.1.124:/home/ec2-user/
                    '''
                }
            }
        }
    }
}
