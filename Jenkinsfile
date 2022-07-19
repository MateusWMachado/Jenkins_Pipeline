pipeline {
    agent any
  
    stages {
        stage('Build') {
            steps {
                sh "git clone https://github.com/MateusWMachado/Jenkins_Pipeline.git"
                sh 'mvn -f Jenkins_Pipeline/spring-boot-hello-world-main -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f Jenkins_Pipeline/spring-boot-hello-world test'
            }
        }
        stage('Deploy') {
            options {
              timeout(time: 20, unit: 'SECONDS')   
            }
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') {
                        try {
                            sh 'docker cp Jenkins_Pipeline/spring-boot-hello-world-main/target/hello-world-1.0.1-SNAPSHOT.jar remote-host:/tmp/new-java-app.jar'
                            sh 'docker container exec remote-host java -jar /tmp/new-java-app.jar'
                        } catch(org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
                            error "Stage interrupted with ${e.toString()}"
                        }
                    }
                }
            }
        }
    }
}
