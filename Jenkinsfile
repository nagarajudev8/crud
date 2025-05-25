pipeline {
    agent any

    tools {
        maven 'Maven 3.9.1'  // Name should match Maven tool configured in Jenkins Global Tools
        jdk 'JDK 11'         // Optional: use appropriate JDK version
    }

    environment {
        MAVEN_OPTS = "-Xmx1024m"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Clean') {
            steps {
                echo 'Running: mvn clean'
                sh 'mvn clean'
            }
        }

        stage('Compile') {
            steps {
                echo 'Running: mvn compile'
                sh 'mvn compile'
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running: mvn test'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Running: mvn package'
                sh 'mvn package'
            }
        }

        stage('Install to Local Repo') {
            steps {
                echo 'Running: mvn install'
                sh 'mvn install'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('fda92abd4eb06e94fd29920be2a026fa680d643c') // Stored credential
            }
            steps {
                echo 'Running: mvn sonar:sonar'
                sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=nagarajudev8_javaapptrend \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running: mvn deploy'
                sh 'mvn deploy'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
