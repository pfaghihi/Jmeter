pipeline {
    agent {
        docker {
            image 'maven:3.9.3-jdk-18'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull your .jmx and pom.xml from Git
                checkout scm
            }
        }

        stage('Run JMeter via Maven') {
            steps {
                // Run JMeter tests using the jmeter-maven-plugin
                sh 'mvn clean verify'
            }
        }
    }

    post {
        always {
            // Publish JMeter HTML report
            publishHTML(target: [
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/jmeter/reports',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report'
            ])

            // Optional: Archive raw .jtl logs
            archiveArtifacts artifacts: 'target/jmeter/results/*.jtl', allowEmptyArchive: true
        }
    }
}