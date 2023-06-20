node {
    checkout scm
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2 --net=host') { c ->
        stage('Build') {
            sh 'pwd'
            sh 'ls'
            sh 'mvn -B -DskipTests clean package'
        }

        stage('Test') {
            try {
                sh 'mvn test'
            } catch (e) {
                echo 'test failed'
                throw e
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Deliver') {
            sh './jenkins/scripts/deliver.sh'
        }
    }
}
