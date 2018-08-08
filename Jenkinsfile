pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build_Docker_Image'){
            when{
                branch 'master'
            }
            steps{
                script{
                    app = docker.build("chaturvedisulabh/train-schedule-new")
                    app.inside {
                        sh 'echo $(curl http://localhost:8080)'
                    }
                }
            }
            stage('Push_Docker_Image'){
                when{
                    branch 'master'
                }
                steps{
                    script{
                        docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login'){
                            app.push("($env.BUILD_NUMBER)")
                            app.push("latest")
                        }
                    }
                }
            }
        }
    }
}
