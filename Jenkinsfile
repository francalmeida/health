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
            environment {
                scannerHome = tool 'SonarQuBe'
            }    
            steps {
                withSonarQubeEnv('SonarQuBe') {
                    bat "${scannerHome}/bin/sonar-scanner"
                }       
            }
        }
    }
}