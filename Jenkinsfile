node {
    stage('Git Checkout'){
        git branch: 'main', url: 'https://github.com/jaydajayda/mcc-auth.git'
    }
    
    stage('Gradle build'){
        sh 'gradle build'
    }
    
    stage('User Acceptance Test') {
        def response = input message: 'Is this build good to go?',
                       parameters: [choice(choices: 'Yes\nNo', description: '', name: 'Pass')]
        
        if(response == "Yes") {
            stage('Deploy') {
                sh 'gradle build -x test'
            }
        }
    }
}