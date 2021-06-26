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
                        bat "\"${scannerHome}/bin/sonar-scanner\""
                    }
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
                echo "Deploying to Heroku"
                bat "git remote -v"
                bat "set"

                //Add Heroku git reference
                bat "heroku git:remote -a morning-ocean-45440" 
                bat "git remote -v"
            }
        }
    }
    post{
        always{
            deleteDir()
        }
    }
}