pipeline{
    agent any
    stages {
    
    stage('build_number') {
        steps {
            buildName '#pipe-${BULID_NUMBER}'
            script{
                currentBuild.displayName ="#pipe-${BUILD_NUMBER}"
            }
        }
    } 
        
      /*
        stage('git') {
            steps {
                git branch: 'J2EE', credentialsId: '7fb1a87f-97b0-40cb-a4c8-6584c4c04407', url: 'https://github.com/pavanballeda/onlinebookstore.git'
                echo 'succesfully cloned git'
            }
        }*/
        stage('build code') {
            steps {
           bat 'mvn clean install'
           echo 'perfectly bulid the code'
            }
        }
    stage('deploy-server') {
        steps {
            
            deploy adapters: [tomcat8(credentialsId: 'e3b16a95-c8db-4769-b263-ee6185bd4110', path: '', url: 'http://localhost:8090')], contextPath: 'pipeline_job', war: '**/*.war'
            echo 'succesfully deployed code'
        }
    }
    stage('nexus-artifactory') {
        steps {
            nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'Maven_host', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target\\onlinebookstore-0.0.1-SNAPSHOT.war']], mavenCoordinate: [artifactId: 'onlinebook', groupId: 'com.online.book', packaging: 'war', version: '1.1.9']]]
            echo 'succesfully push the bulid to artifactory'
        }
    }

    }
}
