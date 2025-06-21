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
failed to query server version call to url while doing jenkins job

sonarqube quality gate is not happening->?
after the sonar scanner is possible, it 
builds the image with execute shell from post build actions-> then we need to setup docker-registry by -> docker run -d -p 5000:5000 --name docker-registry registry:2 -> then images is pushed on localhost:5000 by -> docker tag simple-todo-app:latest localhost:5000/simple-todo-app and then -> docker push localhost:5000/simple-todo-app->then we check it -> docker pull localhost:5000/simple-todo-app -> 
