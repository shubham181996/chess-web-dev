pipeline {
	agnet any
	stages {

	stage ('checkout Source Code'){
	steps {
		checkout SCM
	}
	}
	stage ('Deploy to testing server'){
	steps {
		sh '''
		echo "Uninstall old Apache"
		sudo dnf remove httpd -y
	
		echo "Installing Apache"
		sudo dnf install httpd -y
		sudo systecmctl enable --now httpd
		sudo cp -rf index.html /var/www/html/
		'''	
	}
	}
	stage ('Technical Team Approval'){
	steps {
		input (
		Message: 'Teachnical Team is Testing Sucessfully ?'
		ok: 'Approve Deployment'
		)
	}
	}
	stage ('Deploy to Producation'){
	steps {
		sh '''
		echo "Uninstall old Apache"
                sudo dnf remove httpd -y

                echo "Installing Apache"
                sudo dnf install httpd -y
                sudo systecmctl enable --now httpd
                sudo cp -rf index.html /var/www/html/
                '''
	}
	}
	stage ('Manager Approval'){
	steps {
		input (
		Message: 'Manager Approve Producation Release ?'
		ok: 'Release'
		)
	}
	}
	stage ('Production Release Complete'){
	steps {
		echo "Application Sucessfully Deployed by Jenkins"
	}
	}
	post {
	success {
		emailtext(
		to:'lnxstudy@gmail.com',
		subject: 'Producation Deployment Successfully Done'
		body: '''
		Hello, Team,
		
		Production Deployment Done.

		All Stages Worked.	
		
		'''
	)
	}

	 failure {
                emailtext(
                to:'lnxstudy@gmail.com',
                subject: 'Producation Deployment Failed !'
                body: '''
                Hello, Team,

                Production Deployment Failed.

                All Stages not Worked.

                '''
        )
	}

	}
	}
}
