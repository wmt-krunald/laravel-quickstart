// #!/usr/bin/env groovy

// node {
//     try {
//         stage('build') {
//             // Checkout the app at the given commit sha from the webhook
//             checkout scm

//             // Install dependencies, create a new .env file and generate a new key, just for testing
//             sh "php -r copy('https://getcomposer.org/installer', 'composer-setup.php')"
//             sh "php -r if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
//             sh "php composer-setup.php"
//             sh "php -r unlink('composer-setup.php')"

//             sh "cp .env.example .env"
//             sh "php artisan key:generate"

//             // Run any static asset building, if needed
//             // sh "npm install && gulp --production"
//         }

//         stage('test') {
//             // Run any testing suites
//             sh "./vendor/bin/phpunit"
//         }

//         stage('deploy') {
//             // If we had ansible installed on the server, setup to run an ansible playbook
//             // sh "ansible-playbook -i ./ansible/hosts ./ansible/deploy.yml"
//             sh "echo 'WE ARE DEPLOYING'"
//         }
//     } catch(error) {
//         throw error
//     } finally {
//         // Any cleanup operations needed, whether we hit an error or not
//     }

// }

pipeline {
    agent any
    
    tools {nodejs "node"}

    stages {
        stage('build') {

            steps{
                // Install dependencies, create a new .env file and generate a new key, just for testing
                sh "cp .env.example .env"
                sh "composer update"
                sh "php artisan key:generate"

                // Run any static asset building, if needed
                // sh "npm install && gulp --production"
            }
        }


        stage('deploy') {
            environment {
                NETLIFY_AUTH_TOKEN = credentials('NETLIFY_AUTH_TOKEN')
                NETLIFY_SITE_ID = credentials('NETLIFY_SITE_ID')
            }
            
            steps {
                sh 'npm install -D netlify-cli'
                sh 'npx netlify deploy --site $NETLIFY_SITE_ID --auth $NETLIFY_AUTH_TOKEN --dir build/ --prod'
                echo 'this is main branch'
                // echo "branch is ${params.branch}"
            }
        }
    }     // Any cleanup operations needed, whether we hit an error or not
}




