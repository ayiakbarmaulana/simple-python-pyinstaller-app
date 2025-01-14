pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}



// node {
//   try {

//     docker.image('python:2-alpine').inside {
//       stage('Build') {
//         sh 'python -m py_compile ./sources/add2vals.py ./sources/calc.py'
//       }
//     }

//     docker.image('qnib/pytest').inside {
//       stage('Test') {
//         sh 'py.test --verbose --junit-xml test-reports/results.xml ./sources/test_calc.py'
//       }
//     }

//     docker.image('cdrx/pyinstaller-linux:python2').inside {
//       stage('Deliver') {
//         sh 'pyinstaller --onefile ./sources/add2vals.py'
//       }
//     }
//   } catch (err) {
//     currentBuild.result = 'FAILURE'
//     throw err
//   }
// }