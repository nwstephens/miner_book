# Installation

To use the [miner package](https://github.com/ropenscilabs/miner),
you'll need a Minecraft server that is using the
[RaspberryJuice](https://dev.bukkit.org/projects/raspberryjuice)
plugin. You can [Spigot](https://www.spigotmc.org) to set up your own
server, even locally on your machine. The installation process doesn't
take too long, but it helps to have some _command line_ experience.

## Mac OS X

Installing stuff on a Mac.

## Windows

Installing stuff in Windows.

## Linux

- Make a directory for minecraft and change into it.

  ```
  mkdir ~/minecraft
  cd ~/minecraft
  ```

- Download `Buildtools.jar`

  ```
  wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar
  ```

- Compille it

  ```
  java -jar BuildTools.jar
  ```

- Run it for the first time

  ```
  java -jar -Xms1024M -Xmx2048M spigot-1.12.jar nogui
  ```

- Edit the file `eula.txt` so that the line `eula=false` is instead
  `eula=true`

- Start up the server again. This will take a while (because it's
  building the world), but not as long as the initial compiling of
  `BuildTools.jar`.

  ```
  java -jar -Xms1024M -Xmx2048M spigot-1.12.jar nogui
  ```

  If you need to use a different port, use the `-p` option. ([See other
  options](https://www.spigotmc.org/wiki/start-up-parameters/).)

  ```
  java -jar -Xms1024M -Xmx2048M spigot-1.12.jar -p25566 nogui
  ```

- Type `stop` to stop the server.

- Download the
  [RaspberryJuice plugin](https://www.spigotmc.org/resources/raspberryjuice.22724/)
  by visiting
  [its page](https://www.spigotmc.org/resources/raspberryjuice.22724/)
  and clicking the "Download Now" button in the upper-right. Move this
  `.jar` file to the `~/minecraft/plugins/`

- Edit `~/minecraft/server.properties`.

- Start the server again using the `java -jar .... spigot-1.12.jar`
  command above.



## Docker

### What is Docker?

[Something about Docker and why it's helpful here.]

### The `miner` Dockerfile 

The `miner` package includes a Dockerfile, which is a plain text file that gives Docker the recipe for setting up an appropriate container. 

This file specifies the following steps that are needed to set up the required environment and run a Spigot Minecraft Server with the RaspberryJuice plug-in: 

- Creates a directory called "minecraft" for the Minecraft server
- Downloads all required files to build a Spigot server (https://www.spigotmc.org) and saves them in the "minecraft" direction
- Builds the Spigot server
- Symlink for the built Spigot server?
- Accepts the End User License Agreement for Minecraft ("eula") (see [here](https://account.mojang.com/documents/minecraft_eula) to see what you are agreeing to with this step)
- Downloads the RaspberryJuice plugin (which we're using for API access) to a subdirectory of the "minecraft" directory called "plugins"
- Install the RaspberryJuice plugin
- Open up the ports required to access the game (port 25565) and the API (4711)
- Start the Minecraft server, [explain options we're using for that]

This Dockerfile is included in the `miner` package. To find it on your computer once you've installed the `miner` package, you can run:


```r
system.file("Dockerfile", package = "miner")
```

This call will return the file pathname on your computer for any the file named "Dockerfile" that come with the `miner` package. 

If you'd like to take a look at the Dockerfile, from R you can run: 


```r
edit(system.file("Dockerfile", package = "miner"))
```

This will open the "Dockerfile" file in the `miner` package in a text editor.

### Building a Docker image

The Dockerfile is a very small plain text file and only gives the recipe for setting up the needed environment and starting a server. To get all the required pieces and be ready to run a container, you need to build a Docker image from this Dockerfile. Once you have installed Docker on your computer (which you can do from [the Docker website](https://www.docker.com)), you open a command line (e.g., the Terminal application on MacOS), move into the directory with the Dockerfile (using `cd` to change directory), and then build a Docker image based on this Dockerfile by running the following call from a command line:

```
docker build -t minecraft .
```

The `docker build` call is the basic call to build a Docker image from a Dockerfile. The option `-t minecraft` tells Docker to give the image the tag "minecraft". By doing this, you can later refer to this image as "minecraft". The `.` at the end of the call tells Docker to build this image based on the file called "Dockerfile" in the current working direction (`.`). 

Once you've built the image, you can check to see that it's in the Docker images on your system by running the following call from a command line: 

```
docker images
```

You should see something like this: 

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
minecraft           latest              2c9e2f2c16d3        3 days ago          1.03 GB
java                latest              d23bdf5b1b1b        4 months ago        643 MB
```

This tells you which Docker images you have on your system, when they were created, how large they are, and the Image ID. If you'd ever like to remove a Docker image from your system, you can do that with the command line call `docker rmi` and the image ID. For example, if you ever wanted to remove the "minecraft" image listed above that you built with the call to `docker build`, you could run: 

```
docker rmi 2c9e2f2c16d3
```

### Running a Docker container from the image

Once you have built a Docker image, you can run a container from it. To do that for our Minecraft server, at the command line you should run:

```
docker run -ti --rm -p 4711:4711 -p 25565:25565 minecraft 
```

The `docker run` call is the basic call to run a Docker container from a Docker image. The `minecraft` at the end tells Docker which image to run. The `--rm` option cleans up everything from this container after you're done running it. The `-ti` argument runs the call in the interactive terminal mode. The arguments `-p 4711:4711 -p 25565:25565` allow the needed access to the ports for the game itself (port 25565) and the API (4711).

## Raspberry Pi

Installing stuff on a Raspberry Pi.
