node {
  try {
    docker.image('python:2-alpine').inside {
      stage('Build') {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }

    docker.image('qnib/pytest').inside {
      stage('Test') {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
    }

    docker.image('cdrx/pyinstaller-linux:python2').inside {
      stage('Deliver') {
        sh 'pyinstaller --onefile sources/add2vals.py'
        archiveArtifacts artifacts: 'dist/add2vals'
      }
    }
  } catch (err) {
    currentBuild.result = 'FAILURE'
    throw err
  }
}