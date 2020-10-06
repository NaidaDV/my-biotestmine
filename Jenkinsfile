pipeline {
	agent any
	stages {
		stage("Build") {
			steps {
				sshagent(credentials: ['key-ssh']) {
					sh '''
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "if [ -n $("ps | grep java") ]; then killall java; echo "App not running"; fi"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "if ! [ -d /home/ubuntu/biotestmine ]; then mkdir /home/ubuntu/biotestmine; else rm -R /home/ubuntu/biotestmine; mkdir /home/ubuntu/biotestmine; fi"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "git clone https://github.com/NaidaDV/biotestmine.git /home/ubuntu/biotestmine"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD ./biotestmine/setup.sh
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "sleep 120"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "killall java"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "cd ./biotestmine; nohup ./gradlew cargoRunLocal > /dev/null 1>&2 &"
						ssh -o StrictHostKeyChecking=no -l ubuntu $DEV_IP_JEN_PROD "sleep 120"
					'''
				}
			}
		}
		stage("Test") {
			steps {
				sh '''
				wget -S $DEV_IP_JEN_PROD:8080/biotestmine
				curl $DEV_IP_JEN_PROD:8080/biotestmine
				'''
			}
		}
	}
}
