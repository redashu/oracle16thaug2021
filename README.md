# Docker client options 

<img src="cli.png">

## web UI using portainer

```
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

```


### creating  multi app docker image and container 

```
384  docker  build -t  dockerashu/customer1:v1  . 
  385  cd  customer1/
  386  docker  build -t  dockerashu/customer1:v1  . 
  387  history 
  388  docker  images
  389  history 
  390  docker  run -itd --name ashucapp1  -e  myapp=app1  -p 2211:80  dockerashu/customer1:v1 
  391  docker  ps
  392  docker  run -itd --name ashucapp2  -e  myapp=app2 -p 2200:80  dockerashu/customer1:v1 
  393  docker  ps
  394  docker  run -itd --name ashucapp3  -e  myapp=app3 -p 2299:80  dockerashu/customer1:v1 
  395  docker  ps
  
  ```
  
  
