pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment {
        ArtifactId = readMavenPom().getArtifactId()
        GroupId = readMavenPom().getGroupId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // stage3 : publish to nexus
        stage ('Nexus') {
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: '${ArtifactId}', 
                        classifier: '', 
                        file: 'target/${ArtifactId}-${Version}.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: '620d78dd-6ca3-4fc8-8556-5e386f52ca9d', 
                groupId: '${GroupId}', 
                nexusUrl: '18.222.238.185:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'alissonvisa-snapshot', 
                version: '${Version}'
            }
        }

        stage ('Prinv Env Vars') {
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Group ID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }

        // // Stage3 : Publish the source code to Sonarqube
        // stage ('Sonarqube Analysis'){
        //     steps {
        //         echo ' Source code published to Sonarqube for SCA......'
        //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
        //              sh 'mvn sonar:sonar'
        //         }

        //     }
        // }

        
        
    }

}