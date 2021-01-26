pipeline{
    stages{
        stage('Gradle: Build'){
            steps{
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('Jacoco') {
            steps {
                sh 'build/jacoco/*.exec'
                junit '**/classes'
                step( [ $class: 'JacocoPublisher' ] )
            }
        }

        stage("sonarqube"){
           steps{
                withSonarQubeEnv("teste") {
                    sh './gradlew sonarqube'
                }        
           }
        }s
    }
}