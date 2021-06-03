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
        Packaging = readMavenPom().getPackaging()
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
                script {

                    def NexusRepo = Version.endsWith('SNAPSHOT') ? 'alissonvisa-snapshot' : 'alissonvisa-release'

                    nexusArtifactUploader artifacts: [
                            [
                                artifactId: "${ArtifactId}", 
                                classifier: '', 
                                file: "target/${ArtifactId}-${Version}.${Packaging}", 
                                type: "${Packaging}"
                            ]
                        ], 
                        credentialsId: '620d78dd-6ca3-4fc8-8556-5e386f52ca9d', 
                        groupId: "${GroupId}", 
                        nexusUrl: '3.143.251.240:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: "${NexusRepo}", 
                        version: "${Version}"
                }
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

        // stage ('Deploy') {
        //     steps {
        //         echo "Deploying..."
        //         sshPublisher(
        //             publishers: [
        //                 sshPublisherDesc(
        //                     configName: 'AnsibleController', 
        //                     transfers: [
        //                         sshTransfer(
        //                             cleanRemote: false, 
        //                             excludes: '', 
        //                             execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_tomcat.yaml -i /opt/playbooks/hosts', 
        //                             execTimeout: 120000, 
        //                             flatten: false, 
        //                             makeEmptyDirs: 
        //                             false, 
        //                             noDefaultExcludes: false, 
        //                             patternSeparator: '[, ]+', 
        //                             remoteDirectory: '', 
        //                             remoteDirectorySDF: false, 
        //                             removePrefix: '', 
        //                             sourceFiles: ''
        //                         )
        //                     ], 
        //                     usePromotionTimestamp: false, 
        //                     useWorkspaceInPromotion: false, 
        //                     verbose: false
        //                 )
        //             ]
        //         )
        //     }
        // }

        stage ('Deploy Docker') {
            steps {
                echo "Deploying..."
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'AnsibleController', 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    excludes: '', 
                                    execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                                    execTimeout: 120000, 
                                    flatten: false, 
                                    makeEmptyDirs: 
                                    false, 
                                    noDefaultExcludes: false, 
                                    patternSeparator: '[, ]+', 
                                    remoteDirectory: '', 
                                    remoteDirectorySDF: false, 
                                    removePrefix: '', 
                                    sourceFiles: ''
                                )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false
                        )
                    ]
                )
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