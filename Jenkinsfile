pipeline{
    agent{
        label 'master'
    }
    stages{
        stage('SonarQube - Analysis'){
            steps{
                script {
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQubeServer') {
                        bat "${scannerHome}/bin/sonar-scanner"
                    }
                }
        }
        stage('SonarQube - QualityGates'){
            steps{
                script{
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
        stage('Deploy to Heroku'){
            steps{

            }
        }
    }
}