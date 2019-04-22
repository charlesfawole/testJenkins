pipeline {
    agent any

    stages {

        stage('Run Test') {
            steps {
                dir('testJenkins') {
                    bat 'pip install -e .'

                    timeout (time: 50) {
                        script {
                            try {
                                bat 'python -u scripts/run_link_layer_quality_test.py --config
                                    scripts/config/link_quality_test_config.json
                                    scripts/config/link_quality_test_equipment/%NODE_NAME%.json 2>&1 --output results.csv --rcc_binary programmer.txt --ipg_binary implant.txt'
                                stash name: 'test-results', includes: 'results*'
                                archiveArtifacts artifacts: 'results*'
                            } catch (error) {
                                echo "Failed: ${error}"
                                currentBuild.result = "UNSTABLE"
                            }

                            def exists = fileExists 'results.log'
                            if (exists)
                            {
                                bat 'grep -q "TEST PASSED" results.log'
                            }
                        }
                    }
                }
            }
        }
    }
   
}
