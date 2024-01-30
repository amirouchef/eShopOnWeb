pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'dotnet build eShopOnWeb.sln '
      }
    }

    stage('Tests') {
      parallel {
        stage('Unit') {
          steps {
            sh 'dotnet test tests/UnitTests'
          }
        }

        stage('Integration') {
          steps {
            sh 'dotnet test tests/IntegrationTests'
          }
        }

        stage('Functional') {
          steps {
            warnError(message: 'Functional problem') {
              sh 'dotnet test tests/FunctionalTests'
            }

          }
        }

      }
    }

    stage('Deployement') {
      steps {
        sh ' dotnet publish eShopOnWeb.sln -o /var/aspnet  -p:ErrorOnDuplicatePublishOutputFiles=false'
        dir(path: '/avr/aspnet')
        archiveArtifacts(artifacts: '*', onlyIfSuccessful: true)
      }
    }

  }
}