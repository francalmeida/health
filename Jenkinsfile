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
            steps {
                withSonarQubeEnv('SonarQube Scanner 2.8') {
                bat 'sonar-scanner'
                }
            }
        }
    }
}