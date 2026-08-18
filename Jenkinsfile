pipeline {
    agent any

    stages {
        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to Testing Server') {
            steps {
                sh '''
                echo "Uninstall old Apache"
                sudo dnf remove httpd -y

                echo "Installing Apache"
                sudo dnf install httpd -y
                sudo systemctl enable --now httpd
                sudo cp -rf index.html /var/www/html/
                '''
            }
        }

        stage('Technical Team Approval') {
            steps {
                input(
                    message: 'Technical Team Testing Successfully Done?',
                    ok: 'Approve Deployment'
                )
            }
        }

        stage('Deploy to Production') {
            steps {
                sh '''
                echo "Uninstall old Apache"
                sudo dnf remove httpd -y

                echo "Installing Apache"
                sudo dnf install httpd -y
                sudo systemctl enable --now httpd
                sudo cp -rf index.html /var/www/html/
                '''
            }
        }

        stage('Manager Approval') {
            steps {
                input(
                    message: 'Manager Approve Production Release?',
                    ok: 'Release'
                )
            }
        }

        stage('Production Release Complete') {
            steps {
                echo "Application Successfully Deployed by Jenkins"
            }
        }
    }

    post {
        success {
            emailext(
                to: 'lnxstudy@gmail.com',
                subject: 'Production Deployment Successfully Done',
                body: '''
                Hello Team,

                Production Deployment is complete.
                All stages finished successfully.
                '''
            )
        }

        failure {
            emailext(
                to: 'lnxstudy@gmail.com',
                subject: 'Production Deployment Failed!',
                body: '''
                Hello Team,

                Production Deployment failed.
                Please check the Jenkins console log for details.
                '''
            )
        }
    }
}
