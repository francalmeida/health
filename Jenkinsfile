properties([
  parameters([
    string(name: 'ENV', defaultValue: 'int', description: 'The target environment', ), //use a choices block instead(dev,int,prod)
    //choice(name: 'REGION', choices: ['eu-central-1', 'us-east-1'], description: 'The deployment region', )
   ])
])

pipeline{
    agent {
        node {
            label 'jaws-slaves'
        }
    }

    /*options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
    }*/

    /*environment {
        APP                 = 'netscaler-interface'
        IMAGE_TAG           = "netscaler-latest" 
        REGION = '${params.REGION}'
        REPOSITORY_URI      = env.getProperty("aws_ecr_repository_uri_${params.ENV}_advanced")
        DEPLOYMENT = "cd-afw-${params.REGION}-${params.ENV}-$APP"
        CLUSTER_NAME = "cd-afw-${params.REGION}-${params.ENV}-application-cluster"
        AWS_ECR_CREDENTIALS = "aws_ecr_${params.ENV}_advanced"
        AWS_EKS_CREDENTIALS = "aws_eks_${params.ENV}_advanced"
    }*/

    stages{
        stage('Gradle: Build'){
            tools{
                jdk "JDK 11"
            }

            steps{
                sh 'chmod +x gradlew'
                sh './gradlew clean build -x test --console=plain'
            }
        }

       /* stage('Docker: Build and push image to ECR') {
            steps{
                withAWS(credentials: AWS_ECR_CREDENTIALS, region: params.REGION){
                    script{
                        LOGIN = ecrLogin()
                        sh "$LOGIN"
                        IMAGE = docker.build(REPOSITORY_URI)
                        IMAGE.push(IMAGE_TAG)
                    }
                }
            }
        }*/

        stage('Build') {
            steps {
                sh './jenkins_build.sh'
                junit '*/build/test-results/*.xml'
                step( [ $class: 'JacocoPublisher' ] )
            }
        }

        jacoco(
             execPattern: '**/path_to_file/jacoco.exec',
             classPattern: '**/coverage/**',
             sourcePattern: '**/coverage/**',
             inclusionPattern: '**/*.class'
        )

       /* stage('Kubernetes: Replace running pods') {
            steps {
                withAWS(credentials: AWS_EKS_CREDENTIALS, region: params.REGION) {
                    script{
                        sh "aws eks --region '${params.REGION}' update-kubeconfig --name $CLUSTER_NAME"
                        sh "kubectl rollout restart deployment $DEPLOYMENT"
                    }
                }
            }
        }*/
    }
}