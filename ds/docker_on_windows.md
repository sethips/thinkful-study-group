# Setting up Docker on Windows

These notes may be useful to Windows 10 users attempting to complete the Thinkful Data Science curriculum, Unit 6.8.2 "Docker specialization". If using Unix based distribution is not possible, here is what to do **on windows within a git-bash shell**:

Create an environment variable:
`fpath=c:\\users\\janedoe\\thinkful\\big-data-student-resources`    

The variable name chosen here (`fpath`) is not special; we could have given any valid variable name.

5) Now, we'll run a docker container using this variable. Enter the following command:

`winpty docker run -d --rm -p 8888:8888 -v $fpath:/home/ds/notebooks --name ds thinkfulstudent/pyspark:2.2.1`

Once the container is available locally, Docker will name it `ds` and run it. Let's break down the command line arguments we have here:

- `winpty`: Windows software package providing an interface similar to a Unix pty-master for communicating with Windows console programs. 
- `-d`: This tells the container to run as a single process in the background.
- `--rm`: This tells Docker to remove the container (note, not the underlying image) after we shut it down.
- `-p 8888:8888`: This defines the port mapping from the container to the host. Our container exposes port 8888, and this is the right-side of the colon in the command. The left side (in this case also 8888) indicates the host device port that is associated with the exposed container port.
- `-v $fpath:/home/ds/notebooks`: This command maps the local folder specified by our variable to a corresponding directory inside the container and allows us to save files and data our container needs even after it's done running. The left side of the colon (our previously defined variable) must point to a fully qualified path on the host machine. The right side (`/home/ds/notebooks`) defines a corresponding directory inside the container where the code or data will be mapped.
Note that -v is short for --volume. But, when a pathname is is found on the host side, the persistence layer is just a directory on the host side and is bound via docker bind, not mounted. That is, a volume is created if the hostside pathname is not found and the newly created volume is docker mounted. Unlike binding to a directory, volumes are independent objects that can be copied, cloned, backed up and shared.

- The final argument (`thinkfulstudent/pyspark:2.2.1`) tells Docker the image you wish to run. As we already stated, Docker first looks for the image locally, and if it doesn’t find it, goes to Docker Hub and attempts to pull it down and run it.


6) In your browser, go to `localhost:8888`. You should see the standard Jupyter home screen.

Final note: occasionally, docker on windows stops working after your machine is idle for a while. see [this thread](https://github.com/docker/for-win/issues/1560) for suggested solutions; namely, resetting docker credentials and/or restarting docker.
