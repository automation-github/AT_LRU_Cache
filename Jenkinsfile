pipeline {
  agent any
  stages {
    stage('installation') {
      steps {
            sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
          }
    }
    stage('at_lru_cache_5.1') {
      agent any
      environment {
        test_name = 'test_lru_cache.py'
      }
      steps {
        build(job: 'test_at_lru_cache_5.1', propagate: false)
      }
    }
    
    
    stage('copy xmls') {
      steps {
        sh ''scp -p root@$target_cluster:/tmp/Thumbnails/*.xml . ''
      }
    }
    stage('check_cores') {
      steps {
        sh 'ssh root@$target_cluster "python2.7 /var/Nightswatch/deploy/find_cores.py"'
      }
    }
    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }
  }
  environment {
    target_cluster = '10.65.173.162'
  }
}
