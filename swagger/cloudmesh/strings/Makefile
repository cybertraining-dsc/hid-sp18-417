SWAGGERCODEGEN=java -jar swagger-codegen-cli-2.3.1.jar
YAML=string_init.yaml
DATA=data
DESTDIR=server/
FLASKDIR=server/string/flaskConnexion

dest:
	mkdir $(DESTDIR)

service: 
	$(SWAGGERCODEGEN) generate  -i $(YAML)  -l python-flask  -o $(FLASKDIR)  -D supportPython2=true
	cp -r data $(FLASKDIR)
	cp -r default_controller.py $(FLASKDIR)/swagger_server/controllers

run: 
	cd $(FLASKDIR) && pip install -r requirements.txt && \
	sudo python setup.py install && python -m swagger_server

test:
	curl http://localhost:8080/v2/addString/adding-a-new-string
	curl http://localhost:8080/v2/fetchString/3
	            
stop:
	ps -ef | grep swagger | grep -v "grep"| awk '{print $$2}' | xargs kill -9

clean:
	sudo rm -r $(DESTDIR)