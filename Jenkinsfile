pipeline {
    agent any // <--- MAKE SURE THIS SAYS 'any', NOT 'docker'

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run JMeter via Maven') {
            steps {
                // This 'Maven3' must match the name in Global Tool Configuration
                withMaven(maven: 'Maven3') {
                    sh 'mvn clean verify'
                }
            }
        }
    }


}