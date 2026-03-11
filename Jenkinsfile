pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // This pulls your .jmx and pom.xml from Git
                checkout scm
            }
        }

        stage('Run JMeter Tests') {
            steps {
                // Runs the JMeter Maven Plugin
                // 'verify' triggers the jmeter-maven-plugin goals
                sh 'mvn clean verify'
            }
        }
    }

    post {
        always {
            // This archives the JMeter HTML report so you can see it in Jenkins
            publishHTML(target: [
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/jmeter/reports',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report'
            ])

            // Optional: Archive the raw .jtl log files for debugging
            archiveArtifacts artifacts: 'target/jmeter/results/*.jtl', allowEmptyArchive: true
        }
    }
}