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
        stage ('Checkstyle Analysis') {
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps{
                withSonarQubeEnv('sonar') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vproject-new \
                   -Dsonar.projectName=vproject-new \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
        stage ('Quality Gate') {
            steps{
                timeout(time: 2, unit: 'MINUTES') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }    
            }
        }
    }
}        