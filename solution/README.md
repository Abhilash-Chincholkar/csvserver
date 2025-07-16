Commands I ran during the process
- To download the Images from Repo
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.45.2
1. Run the container image infracloudio/csvserver:latest in background and check if it's running.
- To run the image 
docker run -d --name csvserver infracloudio/csvserver

2. If it's failing then try to find the reason, once you find the reason, move to the next step.
- To check the logs of exited container
docker ps -a
docker logs -f container_name

As now it is clearly shown in logs that inputdata is missing. 

3. Write a bash script gencsv.sh to generate a file named inputFile
I have created script in solution folder, script name is gencsv.sh
it will generate the data reqired.

4. Run the container again in the background with file generated in (3) available inside the container (remember the reason you found in (2)).
Mount the file as volume for container
docker run -d --rm \
  --name csvserver \
  -v "$(pwd)/inputFile:/csvserver/inputdata" \
  infracloudio/csvserver:latest

5. Get shell access to the container and find the port on which the application is listening. Once done, stop / delete the running container.
Login into container - docker exec -it container-name bash and then run netstat -tuln or run directly - docker exec -it csvserver netstat -tuln

6. Same as (4), run the container and make sure,
The application is accessible on the host at http://localhost:9393
Set the environment variable CSVSERVER_BORDER to have value Orange.

docker run -d --rm \
  --name csvserver \
  -v "$(pwd)/inputFile:/csvserver/inputdata" \
  -e CSVSERVER_BORDER=Orange \
  -p 9393:9300 \
  infracloudio/csvserver:latest
