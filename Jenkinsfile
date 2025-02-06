pipeline {
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

//         stage("Nexus Deploy") {
//             steps {
//                 script {
                    // Uncomment the next line to enable Nexus deployment
                    // sh "mvn clean package deploy:deploy -DgroupId=tn.esprit -DartifactId=FirstMavenProject -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.10.114:8081/repository/maven-releases/ -Dfile=target/FirstMavenProject-1.0.jar"
//                     echo 'Nexus deployment done'
//                 }
//             }
//         }

        stage("Nexus Deploy") {
            steps {
                sh """
                    mvn deploy:deploy-file \
                        -DrepositoryId=nexus \
                        -Durl=http://193.95.57.13:8081/repository/maven-releases/ \
                        -Dfile=target/tpAchatProject-1.0.jar \
                        -DgroupId=com.example \
                        -DartifactId=tpAchatProject \
                        -Dversion=1.0 \
                        -Dpackaging=jar \
                        -DgeneratePom=true
                """
            }
        }

        stage("Final Message") {
            steps {
                echo 'Pipeline execution completed successfully!'
            }
        }
    }
}
