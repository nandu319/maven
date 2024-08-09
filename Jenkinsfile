pipeline {
    agent any
    triggers {
      pollSCM '* * * * *'
   }
    stages {
        stage('git') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '965bf949-a5dc-4f47-9858-8b402a09dfb7', url: 'https://github.com/nandu319/maven.git']])
            }
        }

        stage('build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('sonar') {
            steps {
                withSonarQubeEnv(installationName: 'sonar', credentialsId: '7b288055-c2a9-44ac-9169-86962e8cf432') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'studentapp', classifier: '', file: '/var/lib/jenkins/workspace/new/target/studentapp-2.5-SNAPSHOT.war', type: 'war']], credentialsId: '51feef37-7e77-4be3-9c30-36a1c8f6f854', groupId: 'com.jdevs', nexusUrl: '54.87.86.87:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '2.5-SNAPSHOT'
            }
        }

        stage('deploy') {
            steps {
                sh 'scp /var/lib/jenkins/workspace/new/target/studentapp-2.5-SNAPSHOT.war root@18.208.222.9:/var/lib/tomcat/webapps'
            }
        }
    }
}
