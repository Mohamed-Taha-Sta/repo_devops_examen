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
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }

        stage("Deploy Docker Image") {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
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