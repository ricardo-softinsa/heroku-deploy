pipeline{
    agent{
        label 'master'
    }
    environment{
        heroku_login = credentials('heroku_login')
    }
    stages{
        stage('SonarQube - Analysis'){
            steps{echo "analysis"
                /*script {
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQubeServer') {
                        bat "\"${scannerHome}/bin/sonar-scanner\""
                    }
                }*/
            }
        }
        stage('SonarQube - QualityGates'){
            steps{echo "quality gates"
                /*
                script{
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }*/
            }
        }
        stage('Deploy to Heroku'){
            steps{
                echo "Deploying to Heroku"
                bat "git remote -v"
                //bat "set"

                //Add Heroku git reference
                bat "echo ${heroku_login_USR} && echo ${heroku_login_PSW} | \"C:\\Program Files\\heroku\\bin\\heroku\" login"
                bat "\"C:\\Program Files\\heroku\\bin\\heroku\" git:remote -a morning-ocean-45440" 
                bat "git remote -v"
                //Fetch Heroku refs
                bat "git pull heroku main || echo \"t3\""
                //bat "cd .git/refs/remotes/heroku && cat main"

                //Push to heroku
                bat "git push --force heroku HEAD:main"
            }
        }
    }
    post{
        always{
            deleteDir()
        }
    }
}