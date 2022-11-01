pipeline
{
	agent
	{
		label 
		{
			label 'Master'
			
		}
	}
	stages
	{
		stage('Docker-Cleanup')
		{
			steps
			{
				script
				{
					
					def exitCode = sh script: ' sudo docker ps -a | grep -w mytomcat-8081', returnStatus: true
					if ( exitCode == 0 ) 
					{
						sh 'sudo docker stop mytomcat-8081'
					}					
					def exitCode3 = sh script: ' sudo docker images | grep -w mycentos:1.0', returnStatus: true
					if ( exitCode3== 0 ) 
					{
						sh 'sudo docker rmi mycentos:1.0'
						sh 'sudo docker system prune -a -f'
                    			}
					
				}
			}
		}
		stage('Git-Checkout')
		{
			steps
			{
				script
				{
					sh '''
						sudo docker system prune -a -f
						sudo git checkout -f master
						sudo source /root/.bash_profile
            					sudo mvn clean package 
						sudo chmod 755 *
						sudo docker build -t mycentos:1.0 .
						sudo docker run -itdp 8088:8080 --name mytomcat-8081 mycentos:1.0
						sudo chmod 755 gameoflife-web/target/gameoflife.war
						sudo docker cp gameoflife-web/target/gameoflife.war mytomcat-8081:/usr/local/tomcat/webapps
						
						
					'''
				}
			}
		}
	}
}
