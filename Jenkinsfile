pipeline {
    agent any
    triggers {
        cron('H */1 * * * ')
     }
    tools {
        maven 'M2_HOME'
      
    }
    stages {
      stage('Build'){
        steps {
          echo "Build step"
          sh 'mvn clean'
          sh 'mvn install'
          sh 'mvn package'
        }
      
      
      }
        stage('test '){
        steps {
            retry(1){
          echo  "test step"
            }
          sh 'mvn test'
        }
      
      
      }
        stage('deploy'){
        steps {
           sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i /etc/ansible/hosts /etc/ansible/docker-image.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//etc//ansible', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])   
           deploy adapters: [tomcat8(credentialsId: 'tomcatID', path: '', url: 'http://3.87.202.188:8080/')], contextPath: 'slave', war: '**/*.war'
           
        }
      
      
      }
    
    }
    post {
        always {
            echo "Always display this message "
        }
        failure {
            echo "Job failed "
        }
        success {
            echo "Successful run "
        }
        unstable {
            echo "The job is unstable "
        }
    } 

}
