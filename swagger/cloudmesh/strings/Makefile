SWAGGERCODEGEN=java -jar swagger-codegen-cli-2.3.1.jar
YAML=string_init.yaml
DATA=data
DESTDIR=server/
FLASKDIR=server/string/flaskConnexion
all: service start 
	@echo "DONE"

container: docker-build docker-start
	@echo "DONE"

docker-build:

	@echo "building the image from docker file..."
	sudo docker build -t cloudmesh-417/strings .
	@echo "image DONE"

docker-pull:
	@echo "pulling the image from dockerhub..."
	docker pull cloudmesh-417/strings
	@echo "image DONE"

docker-start:
	@echo "starting the service in container..."
	docker run -p 8080:8080 cloudmesh-417/strings

docker-stop:
	@echo "stoping the service..."
	docker stop $$(docker ps -alq)
	@echo "service stopped"

docker-remove:
	@echo "removing the image..."
	docker rmi -f cloudmesh-417/strings
	@echo "image removed"

docker-clean: docker-stop docker-remove
	@echo "DONE"

dest:
	mkdir -p $(DESTDIR)

service:dest 
	wget http://central.maven.org/maven2/io/swagger/swagger-codegen-cli/2.3.1/swagger-codegen-cli-2.3.1.jar
	$(SWAGGERCODEGEN) generate  -i $(YAML)  -l python-flask  -o $(FLASKDIR)  -D supportPython2=true
	rm *.jar
	mkdir $(DATA) 	
	touch $(DATA)/data.csv 	
	cp -r data $(FLASKDIR)
	cp -r default_controller.py $(FLASKDIR)/swagger_server/controllers
	cd $(FLASKDIR) && pip install --upgrade pip; pip install -r requirements.txt && \
	python setup.py install	
start:  
	@echo "starting the service..."
	cd $(FLASKDIR) && python -m swagger_server	

test:
	curl http://localhost:8080/cloudmesh/strings/addString/adding-a-new-string0
	curl http://localhost:8080/cloudmesh/strings/addString/adding-a-new-string1
	curl http://localhost:8080/cloudmesh/strings/addString/adding-a-new-string2
	curl http://localhost:8080/cloudmesh/strings/addString/adding-a-new-string3
	curl http://localhost:8080/cloudmesh/strings/addString/adding-a-new-string4
	curl http://localhost:8080/cloudmesh/strings/fetchString/3
	            
stop:
	ps -ef | grep swagger | grep -v "grep"| awk '{print $$2}' | xargs kill -9

clean:
	sudo rm -r $(DESTDIR)
	sudo rm -r $(DATA)
	rm *.jar
