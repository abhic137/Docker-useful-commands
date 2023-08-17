# Docker-useful-commands
ref: https://linuxhint.com/save-docker-container-as-image/
saving the running docker state:
```
docker commit <container_name>
docker images -a
docker tag <IMAGE_id> <TAG_Name>
docker images -a
```
downloading(saving locally) the image:
```
sudo docker save -o <FILENAME>.tar <REPONAME>

```

loading the image:
```
sudo docker load --input <FILENAME>.tar
sudo docker tag <Image-ID> oai-gnb:latest # May not be needed

```

pushing images in docker hub:
```
sudo docker login
sudo docker images
sudo docker tag <IMAGE_ID> USERNAME/REPONAME:TAG
sudo docker push USERNAME/REPONAME 

```
