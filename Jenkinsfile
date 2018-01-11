podTemplate(label: 'microservices',
    namespace: 'jenkins',
    nodeSelector: 'dev',
    containers: [
        containerTemplate(
            name: 'jnlp',
            image: 'xicoria/jkslave:v1.0'
        ),
        containerTemplate(
            name: 'php',
            image: 'php:7-fpm',
            ttyEnabled: true,
            command: 'cat',
            envVars: [
				envVar(key: 'COMPOSER_ALLOW_SUPERUSER', value: 'true'),
				envVar(key: 'REDIS_HOST', value: 'redis'),
				envVar(key: 'REDIS_PORT', value: '6379')
			]
        ),
        containerTemplate(
			name: 'redis',
			image: 'redis',
            ttyEnabled: true,
            command: 'cat'
		)
    ],
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
) {
      node ('microservices') {
        stage('Setting up php.ini') {
			container('php') {
				sh "echo 'short_open_tag    = On' > /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'variables_order   = GPCS' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'request_order     = GP' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'allow_url_fopen   = On' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'allow_url_include = Off' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'memory_limit        = 512M' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'max_execution_time  = 900' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'max_input_time      = 300' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'post_max_size       = 50M' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'upload_max_filesize = 50M' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'max_input_vars      = 5000' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'always_populate_raw_post_data = -1' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'expose_php          = Off' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'date.timezone = UTC' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'xdebug.remote_enable           = 0' >> /usr/local/etc/php/conf.d/production.ini"
				sh "echo 'xdebug.remote_connect_back     = off' >> /usr/local/etc/php/conf.d/production.ini"
				sh "cat /usr/local/etc/php/conf.d/production.ini"
			}
		}
        stage('Setting up php modules') {
			container('php') {
				sh "yes | pecl install igbinary"
				sh "yes | pecl install redis"
				sh "echo 'extension=igbinary.so' > /usr/local/etc/php/conf.d/extra_php.ini"
				sh "echo 'extension=redis.so' >> /usr/local/etc/php/conf.d/extra_php.ini"
				sh "cat /usr/local/etc/php/conf.d/extra_php.ini"
			}
		}
        stage('Installing composer') {
			container('php') {
				sh "apt-get update"
				sh "apt-get install unzip"
				sh "curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer"
			}
		}
        stage('Updating dependencies') {
			container('php') {
				checkout scm
				sh "cd src"
                sh "ls"
				sh "composer install --prefer-dist --no-autoloader --no-scripts --no-progress --no-suggest"
    			sh "composer clear-cache"
    			sh "composer dump-autoload --classmap-authoritative"
			}
		}
        stage('Running tests') {
            container('php') {
				sh "cd app"
				sh "./vendor/bin/phpunit"
            }
        }	
        stage('Build Docker image') {
            container('jnlp') {
 				checkout scm
                sh "docker build -t test ."
				sh "ls"
            }
        }
    }
}
