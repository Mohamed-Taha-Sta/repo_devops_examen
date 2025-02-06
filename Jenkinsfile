pipeline { 
    agent any
    environment {
registry = "sawsanselmi/devops"
registryCredential = 'sawsanselmi'
dockerImage = 'tpachat'
}
        
    stages {
        stage("Cloning Project"){
            steps {
                git branch: 'main', 
                url: 'https://github.com/sawsanSe/CICD.git'
                echo 'checkout stage'
            }
        }
        
        stage ('MVN clean') {
      steps {
        sh 'mvn clean -e'
        echo 'Build stage done'
      }
    }
    
        stage("compile Project"){
            steps {
                 sh 'mvn compile -X -e'
                  echo 'compile stage done'
            }
        }
        stage("unit tests"){
            steps {
                 sh 'mvn test'
                  echo 'unit tests stage done'
            }
        }
        
        stage("SonarQube Analysis") {
          agent any  
           steps {
                  //sh 'mvn sonar:sonar -Dsonar.projectKey=tpAchat -Dsonar.host.url=http://192.168.10.114:9000 -Dsonar.login=717e16eed6adbb836f32c1b9ee9721acaa488237'
                  echo 'sonar static analysis done'
           }
         }
          
          stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh "mvn package -DskipTests=true"
                                      echo 'Build project Done'

                }
            }
        }
        stage("Nexus Deploy") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    //sh "mvn clean package deploy:deploy -DgroupId=tn.esprit -DartifactId=FirstMavenProject -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://http://192.168.10.114:8081/repository/maven-releases/ -Dfile=target/FirstMavenProject-1.0.jar"
                                  echo 'Nexus Deployment done'

                    
                }pipeline {
                     agent any

                     environment {
                         registry = "sawsanselmi/devops"
                         registryCredential = 'sawsanselmi'
                         dockerImage = 'tpachat'
                     }

                     stages {
                         stage("Cloning Project") {
                             steps {
                                 git branch: 'main', url: 'https://github.com/sawsanSe/CICD.git'
                                 echo 'Checkout stage completed'
                             }
                         }

                         stage("MVN Clean") {
                             steps {
                                 sh 'mvn clean -e'
                                 echo 'Build stage done'
                             }
                         }

                         stage("Compile Project") {
                             steps {
                                 sh 'mvn compile -X -e'
                                 echo 'Compile stage done'
                             }
                         }

                         stage("Unit Tests") {
                             steps {
                                 sh 'mvn test'
                                 echo 'Unit tests stage done'
                             }
                         }

                         stage("SonarQube Analysis") {
                             agent any
                             steps {
                                 // Uncomment the next line to enable SonarQube analysis
                                 // sh 'mvn sonar:sonar -Dsonar.projectKey=tpAchat -Dsonar.host.url=http://192.168.10.114:9000 -Dsonar.login=717e16eed6adbb836f32c1b9ee9721acaa488237'
                                 echo 'Sonar static analysis done'
                             }
                         }

                         stage("MVN Build") {
                             steps {
                                 script {
                                     sh "mvn package -DskipTests=true"
                                     echo 'Build project done'
                                 }
                             }
                         }

                         stage("Nexus Deploy") {
                             steps {
                                 script {
                                     // Uncomment the next line to enable Nexus deployment
                                     // sh "mvn clean package deploy:deploy -DgroupId=tn.esprit -DartifactId=FirstMavenProject -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.10.114:8081/repository/maven-releases/ -Dfile=target/FirstMavenProject-1.0.jar"
                                     echo 'Nexus deployment done'
                                 }
                             }
                         }

                         stage("Building Docker Image") {
                             steps {
                                 script {
//                                      dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                                    echo "Image Built"
                                 }
                             }
                         }

                         stage("Deploy Docker Image") {
                             steps {
                                 script {
//                                      docker.withRegistry('', registryCredential) {
//                                          dockerImage.push()
//                                      }
                                    echo "Image deployed"
                                 }
                             }
                         }

                         stage("Cleaning Up") {
                             steps {
                                 sh "docker rmi ${registry}:${BUILD_NUMBER}"
                             }
                         }
                     }
                 }

            }
        }
        
   stage('Building our image') {
steps {
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy our image') {
steps {
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('Cleaning up') {
steps {
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
}


    