node {
    checkout scm
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2 --net=host') { c ->
        stage('Build') {
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

        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahapan deploy? (Klik Proceed untuk melanjutkan)'
        }

        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep(time: 1, unit: 'MINUTES')
        }
    }
}
