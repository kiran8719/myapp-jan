pipeline{
    agent any
    triggers {
      pollSCM '* * * * *'
    }
    stages{
        stage("Maven Build"){
            when {
                branch "develop"
            }
            steps{
               sh "mvn package"
            }
        }
        stage("SonarQube Analysis"){
            when {
                branch "develop"
            }
            steps{
               withSonarQubeEnv('sonar7') {
                    sh "mvn sonar:sonar"
                }
            }
        }
        stage("sonar status"){
            when { 
                branch "develop"
            }
            steps{
                    script{
                        timeout(time: 1,unit: 'hour'){
                        def qg = waitforQualitygate()
                        if (qg.status!='ok'){
                            error "pipeline aborted due to quality gate failure : ${qg.status}"
                        }
                    }
                }
            }
        }
        stage("Nexus"){
            when {
                branch "develop"
            }
            steps{
               echo "uploading artifacts to nexus...."
            }
        }
        stage("Deploy To QA"){
            when {
                branch "qa branch"
            }
            steps{
               echo "deploying to qa server init...."
            }
        }
        stage("Deploy To production"){
            when {
                branch "master"
            }
            steps{
               echo "deploying to master server...."
            }
        }
    }
}