pipeline {
    agent any
    triggers{
        cron('3 * * * *')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Cloning git repertory and building docker services'
                sh 'whoami'
                sh "rm -rf AutomationTest"
                sh 'git clone https://github.com/tnunez85/AutomationTest.git'
                sh 'docker-compose up -d'
            }
            post {
                failure {
                    echo 'Something weird happened with Docker. Need to review.'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Smoke test will be submitted'
                sh 'cd AutomationTest'
                sh 'mvn test -Dcucumber.options="--tags @smoke"'
                            }
            post{
                failure{
                 echo 'Test exited with error'
                 sh 'docker-compose down'
                }
            }
        }
        stage('Bye Bye!') {
            steps {
                sh 'docker-compose down'
                echo 'bye-bye docker!!'
            }

        }

    }
}

