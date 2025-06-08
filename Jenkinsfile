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
      POM_VERSION = readMavenPom().getVersion() // readMavenPom() reads the pom the xml file, getVersion() get version will display version from reading the pom.xma and stores in "POM_VERSION" 
      POM_PACKAGING = readMavenPom().getPackaging() // it will display 'jar','var' file and stores in "POM_Pacakaging"
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
                -Dsonar.host.url="${env.SONAR_URL}" \
                -Dsonar.login=${SONAR_TOKEN}
            """
          
        }
        timeout (time: 2, unit: 'MINUTES'){
            waitForQualityGate abortPipeline: true
        }
      }
    }

    stage('Docker Build'){
      steps{
        // for this we need to install "pipeline-utility-steps" plugin
        // it will just printnot change the file name
        //My Articate file name looks like : i27-eureka-0.0.1-SNAPSHOT.Jar
        echo "My Jar Sorce like: i27-${env.Application_Name}-${env.POM_VERSION}.${env.POM_PACKAGING}"
        // I want to see my rtifact name like this: i27-eureka-20-main.jar
        echo "Required display name like this: i27-${env.Application_Name}-${BUILD_NUMBER}-${BRANCH_NAME}.${env.POM_PACKAGING}"
      }
    } 
  }

}
