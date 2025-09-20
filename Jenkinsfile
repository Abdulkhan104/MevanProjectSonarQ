pipeline {
    agent any
    
    environment {
        SONARQUBE = 'sonar-server'  
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Abdulkhan104/MevanProjectSonarQ.git'
            }
        }
        
        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }
        
        stage('Code Quality') {
            steps {
                withSonarQubeEnv(SONARQUBE) {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    echo "Quality Gate Response: ${qg}"  // Debugging output

                    if (qg.status != 'OK') {  // Fail only if QG is not OK
                        error "❌ Pipeline failed due to not meeting SonarQube Quality Gate requirements."
                    } else {
                        echo "✅ Quality Gate Passed!"
                    }
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Build and SonarQube analysis completed successfully with passing Quality Gates.'
        }
        failure {
            echo '❌ Build or SonarQube Quality Gate validation failed.'
        }
    }
}
