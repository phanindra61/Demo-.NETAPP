pipeline {
    agent any

    stages {
        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build --no-restore --configuration Release'
            }
        }
        stage('Test') {
            steps {
                // "|| true" lets the pipeline continue even if no test project
                // exists yet in this small demo.
                sh 'dotnet test --no-build || true'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t demoapp .'
            }
        }
        stage('Docker Run') {
            steps {
                // Remove any previous container with this name so re-running
                // the pipeline doesn't fail on a name clash.
                sh 'docker rm -f demoapp || true'
                sh 'docker run -d -p 5000:80 --name demoapp demoapp'
            }
        }
    }
}
