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

       stage('Sonarqube') {
            steps {
            withSonarQubeEnv('SonarScanner') {
                bat "${scannerHome}/bin/sonar-scanner"
            }        timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
         }
        }
    }
}