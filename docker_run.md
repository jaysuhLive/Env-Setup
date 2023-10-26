# Install docker for ubuntu18.04

- 1. system repo update
```
sudo apt update
```
- 2. install dependencies 
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
- 3. add docker repository key 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
````
- 4. add docker repository 
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
- 5. system repo update 
```
sudo apt update
```
- 6. install docker with apt, check available or installed docker version with command
```
apt-cache policy docker-ce
sudo apt install docker-ce
```
- 7. add group
```
sudo usermod -aG docker $USER
```
## check if docker is well installed and running
```
sudo systemctl status docker 
```
