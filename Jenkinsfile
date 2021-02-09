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

        stage('SonarQube analysis', envOnly:true) {
            steps {
                withSonarQubeEnv('sonarQube') {
                 bat './gradlew sonarqube'
                 println ${env.http://localhost:9000"}
                }
            }
        }
    }
}