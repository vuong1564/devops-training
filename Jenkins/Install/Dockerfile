FROM jenkins/jenkins:lts

USER jenkins

# copy plugins.txt to the $REF/init.groovy.d directory
COPY plugins.txt /usr/share/jenkins/ref/init.groovy.d/plugins.txt
# install all plugins listed up there
#RUN jenkins-plugin-cli < /usr/share/jenkins/ref/init.groovy.d/plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/init.groovy.d/plugins.txt

USER jenkins