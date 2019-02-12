Here's what to do **on windows**:

1) Head over to GitHub and fork the [big data student resources repository](https://github.com/Thinkful-Ed/big-data-student-resources) to your account.

2) Clone your new repo to your local environment using `git clone`. Make a note of the path you've cloned the repo to because you'll need it again soon.

3) Before doing anything else, make sure you don't have any previous Jupyter Notebooks running on port 8888 (the default port Notebooks run on). We'll make the Jupyter Notebook instance served by our Docker container available on that port.

4) From the command line, store the  local path to the repo you just cloned *as a variable*; however, in order for windows to interpret this variable correctly, the usual forward slash must instead be a double backslash. For example, if the path is ***c:/users/janedoe/thinkful/big-data-student-resources***, run:

`fpath=c:\\users\\janedoe\\thinkful\\big-data-student-resources`    
The variable name chosen here (`fpath`) is not special; we could have given any valid variable name.

5) Now, we'll run a docker container using this variable. Enter the following command:

`winpty docker run -d --rm -p 8888:8888 -v $fpath:/home/ds/notebooks thinkfulstudent/pyspark:2.2.1`

Let's unpack this command. It tells Docker to run a container called `thinkfulstudent/pyspark:2.2.1`. Docker will first check to see if this container is available locally. If so, it will run the container. If not, it will try to download it from Docker Hub, which is what will happen this time around. Unless you have insanely fast Internet, this will take a few minutes.

Once the container is available locally, Docker will name it `ds` and run it. Let's break down the command line arguments we have here:

- `winpty`: **--Mike Ricos, you may need to add this explanation in**
- `-d`: This tells the container to run as a single process in the background.
- `--rm`: This tells Docker to remove the container (note, not the underlying image) after we shut it down.
- `-p 8888:8888`: This defines the port mapping from the container to the host. Our container exposes port 8888, and this is the right-side of the colon in the command. The left side (in this case also 8888) indicates the host device port that is associated with the exposed container port.
- `-v $fpath:/home/ds/notebooks`: This command maps the local folder specified by our variable to a corresponding directory inside the container and allows us to save files and data our container needs even after it's done running. The left side of the colon (our previously defined variable) must point to a fully qualified path on the host machine. The right side (`/home/ds/notebooks`) defines a corresponding directory inside the container where the code or data will be mapped.
- The final argument (`thinkfulstudent/pyspark:2.2.1`) tells Docker the image you wish to run. As we already stated, Docker first looks for the image locally, and if it doesn’t find it, goes to Docker Hub and attempts to pull it down and run it.

6) After the container starts running, you should see the ID of the new container printed to the command line.

7) Try running the command `docker container ls` to get info about any currently running containers.

[same image as in curriculum]

8) In your browser, go to `localhost:8888`. You should see the standard Jupyter home screen. If your notebooks directory mapped properly, it will be populated with the notebooks in that directory when you launch, as shown below. If the Jupyter home screen appears but this directory shows up empty, recheck your local directory variable (noted as `fpath` above). 

[same image as in curriculum]


Final note: occasionally, docker on windows stops working after your machine is idle for a while. see [this thread](https://github.com/docker/for-win/issues/1560) for suggested solutions; namely, resetting docker credentials and/or restarting docker.
