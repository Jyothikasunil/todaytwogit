pipeline {
    agent any

    environment {
        RECIPIENTS = 'jyothikasunil006@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Specify your build tool, e.g., Maven
                    bat 'mvn clean package'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Specify your test automation tools, e.g., JUnit
                    bat 'mvn test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Specify your code analysis tool, e.g., SonarQube
                    withSonarQubeEnv('SonarQube') {
                        bat 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Specify your security scan tool, e.g., OWASP Dependency-Check
                    bat 'dependency-check.bat --project MyProject --scan ./'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging...'
                    // Specify deployment steps, e.g., AWS CLI
                    bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name StagingGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Example of running custom integration test script
                    bat 'run-staging-tests.bat'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production...'
                    // Specify production deployment steps, e.g., AWS CLI
                    bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name ProductionGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip'
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Pipeline finished. Sending notification emails...'
            }
        }
        success {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - SUCCESS",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - FAILURE",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
    }
}
