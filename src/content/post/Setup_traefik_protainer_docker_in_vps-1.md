---
title: "Setup Traefik + Portainer on a VPS & Docker"
publishDate: "20 August 2023"
description: "Tutorial walkthrough to help you setup your own server with vps, traefik and portainer"
tags: ["tutorial", "cloud"]
---

> on one of my quite weekends, i had nothing to do. Then i decided to try something with my laptop. Long story short i ended up spent the whole weekend to finish what i have started. I'll put all of them here so you can easily replicate it without having to spent too much time just like me. here we go!


## Intro.
Cloud system for me is something that i rarely touch. as a fullstack engineer who work on Desktop app development for almost all of my entire career, i have no experience about developing and deploying a software to a cloud platform. if you a programmer just like me that have similar condition, this article is for you.

We have plenty cloud platforms nowadays, the big three are AWS, Azure, and Google Cloud Platform. I will not talk about those three. I just want to build something personal (cheap) here. Let's talk about something more side-project-friendly like Digital Ocean and Linode.

before we start, make sure you have several setup below, and if you don't, please spent your time (and money) little bit so we can move on.

## Domain.
Have a Domain! Well, it's not a necessary, but it would help us more. or at least we can make a personal endpoint url. you can purchase it anywhere like namecheap.com or your local domain registrar. we will use the DNS management tools to add some of our A records.

the list of records that we need to add are,
- traefik.`<you domain name>.<your top-level domain>`
- portainer.`<you domain name>.<your top-level domain>`
- db.`<you domain name>.<your top-level domain>`

## Cloud Platform.
Choose one from either Digital Ocean or Linode. Based on my personal research, both of them have the most convenience experience and not so pricy (of course!). Purchase the basic (i mean the cheaper) tier because we don't need much processing here.


that's all things that you need to purchase, believe me. other than those two, the rest are free. So, no more excuse, let's dig in!

## Fun part.
there are several steps that we need to setup. Hang on, we will walkthrough it together. 

## Docker.
we want to deploy our system with docker, why? because it's going to give us plenty of benefit such as isolated environment and easy to setup. So, let's start with installing docker.

if you're new with docker, go to YouTube and suit yourself and watch docker explanation first. in short, we want to make every tool on our VM in a container rather than installing their prerequisites one by one. dockerized tools can save us time. this walkthrough is for Ubuntu VM so if you another linux distro, it should be the same, i guess.
- open your VM console terminal, then first, we need to install some packages to configure docker's repository.

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
```

- after all the packages are successfully installed, then add docker's GPG key like below.
```
for ubuntu distro:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
for debian distro:
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

- now we are ready to download docker repository!
```
for ubuntu distro:
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
for debian distro:
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- and now the last thing, let's install them!
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

and yeah, we are ready now, but before going I'll put some stuff just in case you do mistakes along the way, some miss typing or miss pasting the script.

If you put GPG key and the put the wrong command in source.list.d
this happened to me, and this gave me error when i did sudo apt update. so what need to do is to delete everything that i had done.
delete GPG key

```
sudo rm -r -f /usr/share/keyrings/docker-archive-keyring.gpg
```

delete all list under source.list.d
```
cd /etc/apt/sources.list.d/
sudo rm -r -f <all stuff located under this directory one by one>
```

and re-do the step from the top once again, this time make sure you do it right.

### Docker installation completed.
so now we have docker installed in our VM machine. but we are not finished yet. there is a one more tool that we need to setup and still related to docker, it is docker compose.