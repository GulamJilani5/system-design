# Container vs VM
### Containers are based on OS virtualization(Multiple containers can run on same OS)
### VMs virtualize the hardware, creating a complete virtual computer with its own operating system.


# Linux vs MacOS/Windows
### Linux
##### Linux Features
- namespace and cgroup are the features of Linux only.
- **namespace** allows for the creation of isolated environment within the OS. 
- Container has its own set of namespace including `process`, `network`, `mount` and `IPC`(inter process communication).
- **cgroup** provides resource management and allocations like `cpu`, `memory`, `disk I/O` and `network`.

##### Installing Docker on Linux
- Complete **Docker Engine** is installed on host Linux OS.


##### Installing Docker on MacOS or Windows
- **Docker Client** is installed on host MacOS/Windows.
- A lightweight VM is configured with Linux and **docker server** is installed within that VM.


# How docker architectures are communicating.
- 1. Instructions are given from docker client to docker server to run the container.
- 2. Docker searches the image in host(local) system if not found then searches in the `docker registry`.
- 3. **Docker Server** pulls the image from registry to local.
- 4. **Docker Server** creates a running container from the image.
- 5. This running container is our microservice or web application.

`Image (static) → docker run → Container (running, live microservice/app)`




