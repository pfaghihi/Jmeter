pipeline {
    agent any
// This section creates the input field in the Jenkins UI
//     parameters {
//         string(name: 'THREADS', defaultValue: '1', description: 'How many virtual users to run?')
//        // string(name: 'HOST', defaultValue: 'host.docker.internal', description: 'Target Server Host')
//     }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run JMeter via Maven') {
            steps {
                withMaven(maven: 'Maven3') {
                    // In Jenkins/Linux sh, we don't need the extra quotes for dots
                    // Changed localhost to host.docker.internal for Docker-to-Host connectivity
                    sh 'mvn clean verify -Djmeter.protocol=http -Djmeter.host=localhost -Djmeter.threads=2'

                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/jmeter/results/*.jtl', allowEmptyArchive: true

            publishHTML(target: [
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/jmeter/reports',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report'
            ])
        }
    }
}