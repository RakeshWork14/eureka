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
        sh """
        echo "starting sonar scan"
        mvn sonar:sonar \
            -Dsonar.projectKey=i27-eureka \
            -Dsonar.host.url=http://34.29.155.140:9000 \
            -Dsonar.login=squ_0f32ef0ac2b39b853051e1b9b00c7f370cfe56cd
        """
      }
    }
  }

}
