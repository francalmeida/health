pipeline{

    agent any
    
    stages{
        stage('Gradle: Build'){
            steps{
                bat './gradlew build'
            }
        }

        stage('Jacoco') {
            steps {
                bat 'build/jacoco/*.exec'
                junit '**/classes'
                step( [ $class: 'JacocoPublisher' ] )
            }
        }

        stage("sonarqube"){
           steps{
                withSonarQubeEnv("teste") {
                    bat './gradlew sonarqube'
                }        
           }
        }
    }
}