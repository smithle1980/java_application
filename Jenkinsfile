pipeline{
    agent any
    
    tools{
        maven 'Maven-David'
    }
    
    stages{
        stage('Git Clone'){
            steps{
                git branch: 'main', credentialsId: 'my-github-credentials', url: 'https://github.com/smithle1980/java_application.git'
            }
        }
        
        stage('Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        
        stage('DavidSonar'){
            environment{
                scannerHome = tool 'Sonar'
                
            }
            steps{
                withSonarQubeEnv('SonarServer'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=david_java_sonar -Dsonar.sources=. -Dsonar.java.binaries=target/classes/com/mycompany/app/ "
                }
            }
        }
        
        stage('Quality Gate'){
            steps{
                waitForQualityGate abortPipeline: true 
            }
        }
        
        stage('Deploy'){
            steps{
                s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: 'david-new-bucket', excludedFile: '', flatten: false, gzipFiles: false, keepForever: false, managedArtifacts: false, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: 'target/mywork3-1.0-SNAPSHOT.jar', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false]], pluginFailureResultConstraint: 'FAILURE', profileName: 'AWS-S3', userMetadata: []
            }
        }
    }
}
