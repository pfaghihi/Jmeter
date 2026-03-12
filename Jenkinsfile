pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run JMeter') {
            steps {
                withMaven(maven: 'Maven3') {
                    // Running with the parameters passed correctly
                    sh 'mvn clean verify -Djmeter.protocol=http -Djmeter.host=host.docker.internal -Djmeter.port=8080 -Djmeter.threads=2'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/jmeter/results/*.jtl', allowEmptyArchive: true

            // This is safer if the plugin might be missing
            script {
                if (isUnix()) {
                    echo "Check the target/jmeter/reports folder for results."
                }
            }
        }
    }
}