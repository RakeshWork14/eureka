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
  }

}
