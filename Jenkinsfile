pipeline{
    agent{
        label 'slave-1'
    }

    tools{
      jdk  'JDK-17'
      maven 'maven-8.8'
    }

    environment{
      Application_Name = "eureka"
      SONAR_TOKEN = credentials('sonar_creds')
      SONAR_URL = "http://34.29.155.140:9000"
    }
  stages{
    stage('buildstage'){
        steps{
            echo "building the ${env.Application_Name} Application"
            sh 'mvn clean package -DskipTests=true'
        }
    }

    stage('sonarstage'){
      steps{
        echo "starting sonar scan"
        withSonarQubeEnv('sonar'){  // This name is you saved under manage jenkins in sonar
            sh """
            mvn sonar:sonar \
                -Dsonar.projectKey=i27-eureka \
                -Dsonar.host.url= "${env.SONAR_URL}"  
                -Dsonar.login=${SONAR_TOKEN}
            """
          
        }
        timeout (time: 2, unit: 'MINUTES'){
            waitForQualityGate abortPipeline: true
        }
      }
    }
  }

}
