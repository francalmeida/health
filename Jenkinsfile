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
               jacoco( 
                    execPattern: "build/jacoco/*.exec",
                    classPattern: '**/classes',
                    sourcePattern: '**/src/main/java',
                    inclusionPattern: '**/*.java,**/*.groovy,**/*.kt,**/*.kts'
                )
            }
        }

        stage('SonarQube analysis') {
            tools {
                sonarQube 'SonarQube Scanner 2.8'
            }
            steps {
                withSonarQubeEnv('SonarQube Scanner') {
                bat 'sonar-scanner'
                }
            }
        }
    }
}