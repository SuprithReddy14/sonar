pipeline {
    agent any
    stages {

        // CI Start
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }

        stage("SonarQube analysis") {
            when {
                anyOf {
                    branch 'feature/*'
                    branch 'main'
                }
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    try {
                        timeout(time: 10, unit: 'MINUTES') {
                            waitForQualityGate abortPipeline: true
                        }
                    }
                    catch (Exception ex) {

                    }
                }
            }
        }
        stage('Push') {
            steps {
                echo 'Push'
            }
          }
        // Ci Ended

        // CD Started

       
        stage("Deployment") {
            parallel {
                stage('Deploy to Dev') {
                    steps {
                        echo 'Build'
                    }
                }

                stage('Deploy to test ') {
                    when {
                        branch 'main'
                    }
                    steps {
                        echo 'Build'
                    }
                }
            }
        }
            
    }
        // CD Ended
}
