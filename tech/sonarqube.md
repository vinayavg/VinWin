https://www.sonarqube.org/

https://hub.docker.com/_/sonarqube

SonarQube is an open source platform for continuous inspection of code quality.

docker run -d --name sonarqube \
    -p 9000:9000 \
    -e sonar.jdbc.username=sonar \
    -e sonar.jdbc.password=sonar \
    -e sonar.jdbc.url=jdbc:postgresql://localhost/sonar \
    sonarqube
    
Option 4: Customized image
In some environments, it may make more sense to prepare a custom image containing your configuration. A Dockerfile to achieve this may be as simple as:

FROM sonarqube:7.4-community
COPY sonar.properties /opt/sonarqube/conf/
You could then build and try the image with something like:

$ docker build --tag=sonarqube-custom .
$ docker run -ti sonarqube-custom


https://www.vogella.com/tutorials/SonarQube/article.html

$ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube

mvn sonar:sonar   -Dsonar.host.url=http://localhost:9000   -Dsonar.login=yourkey

mvn sonar:sonar -Dsonar.analysis.mode=preview -Dsonar.issuesReport.html.enable=true

https://docs.sonarqube.org/latest/

