podTemplate(containers: [ 
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk14',
        command: 'sleep',
        args: '30d'
),
]) {   
  node(POD_LABEL) {
    stage('Build a gradle project') {

      // This was a test run in my old repo using the main branch
      git branch: 'main', credentialsId: 'jenkins-user-token', url: 'https://github.com/AshleeGhil/week6.git'
      container('gradle') {
        stage('Build a gradle project') {
         // My gradle is in the top level of this particular repo so no cd was necessary
          sh '''
          ./gradlew build
          mv ./build/libs/calculator-0.0.1-SNAPSHOT.jar /mnt
          '''
        }
      }
    }

    stage('Build Java Image') {
      container('kaniko') {
        stage('Build a gradle project') {
          sh '''
          echo 'FROM openjdk:8-jre' > Dockerfile
          echo 'COPY ./calculator-0.0.1-SNAPSHOT.jar app.jar' >> Dockerfile
          echo 'ENTRYPOINT ["java", "-jar", "app.jar"]' >> Dockerfile
          mv /mnt/calculator-0.0.1-SNAPSHOT.jar .
          /kaniko/executor --context `pwd` --destination AshleeGhil/hello-kaniko:1.0
          '''
        }
      }
    }

  }
}

  }
}