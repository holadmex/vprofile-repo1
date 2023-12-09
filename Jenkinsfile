pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('FETCH THE CODE FROM GITHUB') {
            steps{
                git branch: 'vp-rem', credentialsId: 'gitlogin', url: 'git@github.com:holadmex/vprofile-repo1.git'
            }
        }
        stage ('BUILD THE CODE') {
            steps{
                sh 'mvn clean install -DskipTest'
            }
            post {
                success {
                    echo 'Archiving artifacts now.'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
        }
    }
}        