Containers are the new technologies in the enterprise IT, and most seem to be very interested in this technology. Many are using them to develop and deploy applications to the public. Containers are useful for many reasons. They isolate applications and operating systems from the rest of the system, thus bringing speed and flexibility. They are portable and easy to clone and move to other operating systems. Thus, containers can be more or less of a virtualized software environment that the application or operating system runs on a host computer.

There several kinds of container technologies you can you. These include Docker containers, Kubernetes containers, and Linux containers (LXC). This article will go over and compare LXC over Docker.

### What is LXC, and what does it do

[LXC](https://linuxcontainers.org/LXC/introduction/) is a userspace interface for the [Linux kernel containment features](https://linuxcontainers.org/LXC/getting-started/). It uses a sophisticated API and simple tools to make it easy for Linux users to develop and maintain systems or application containers.

To understand LXC well, let first discuss what a virtual machine is.

#### The concept of Virtual Machines (VM)

Typically, in a virtual machine, you have a host machine where you install your operating system. It could be Windows, macOS, or Linux on the host machine. When you use a virtual machine, you add or install hypervisors in your host operating system, for example, VirtualBox, VMware, and ESXi. You use the hypervisor to install a guest operating system to run on top of your host operating system. In this case, the guest OS is running as a virtual machine/container. Each of the guest operating systems you add is a complete operating system. all the hardware resources (RAM, CPU, HDD/SSD) attached to these guest virtual machines are virtualized. And the hypervisor is responsible for allocating the guest OS the hardware resources they need from your base/host machine in a virtualized fashion. Basically, you create an instance of an emulator that you can interact with just like an ordinary computer. The image below depicts this concept.

![Virtual machine architecture](/engineering-education/lxc-vs-docker-what-is-the-difference-and-why-docker-is-better/virtual-machine-VM-architecture.jpeg)

[Image source](https://www.docker.com/resources/what-container)

The main issue with the above is that every instance running will need resource allocation, which can lead to over-allocation of the available resources. Every instance of an operating system has to have these resources allocated to it, and those resources will be used whether or not the server needs them.

Check this out. If you have 16GB of RAM and start 4 Guest machines/servers, they will use all of the RAM that is currently available, i.e., each is allocated 4GB of ram. Even if the individual server only uses 1GB of RAM. Assuming each can run on 1GB of RAM, you just need 4RAM to boot up these four virtual hosts. But because the instances that they are in have been given four gigs each, they are using 16GB of ram within your virtual environment. If you were to add another virtual host, you wouldn't have enough RAM to start it. Every time a VM starts, all of the resources that have been assigned to it are automatically given to it. Those resources can't be assigned to another guest host. In the end, you have lots of wasted resources.

Another issue is that all the guest operating instances are running on a single machine. In case of any boot-up processes, or whenever you want to restart service for some reason, you have to reboot that entire computer. depending on what environment you are you're in, a couple of extra seconds matter

#### LXC virtual environments (VE)

On the other hand, [LXC](https://www.youtube.com/watch?v=CWmkSj_B-wo) (Linux Containers) is an operating system-level virtualization solution that enables the creation and operation of many isolated Linux virtual environments (VE) on a single centralized host. Containers, which are isolation levels, can isolate certain apps or simulate a fully different host.

LXC mitigates VM disadvantages. Linux containers enable the host CPU to effectively divide memory allocations into confinement levels known as namespaces. In comparison, a VM contains the whole operating system and machine setup/emulators, such as the hard disk, virtual CPUs, and network interfaces. Its whole enormous virtualized environment typically takes some time to boot and consumes a large amount of RAM and CPU. LXC virtual environment has no hardware preload emulation. Each virtual environment (an OS or an application) is loaded in a container and executes without any additional overhead and no hardware emulation. This means no penalty from software with limited memory. In the end, LXC will improve the performance of the bare metal as it only bundles the OS/application that is required.

![LXC container](/engineering-education/lxc-vs-docker-what-is-the-difference-and-why-docker-is-better/lxc-container.png)

[Image source](https://www.polarsparc.com/xhtml/LXC.html)

In simple terms, in LXC virtual environments (VE), you create containers for the service the virtual OS, application, or server wants to provide. At the same time, the underlying hardware resources and kernel is shared from all the containers. For example, Let's say you have a container running a LAMP server (Linux, APACHE, MYSQL PHP). In this case, the software installed, the configuration files, and the IP address are all virtualized. However, the container doesn't have the kernel and the RAM (hardware resources) directly associated with it. It is operating on the lower level of the base metal.

In other terms, Let's say you have three different containers, I.e., a LAMP server, a DHCP container, and a DNS container. In this case, all the configuration files are contained within this container. And they all share the underlying resources. In this case, resource allocation will be much easier as compared to VMs. If the LAMP server needs alot of resources, it can be allocated to it. So if the DNS container does need alot of RAM now, and the LAMP server is demanding additional RAM now, that memory can be assigned to the LAMP server to speed up this container's services. Resources are allocated according to the need of every container.

Another important thing to note is that each container is contained within itself with its configuration files. Each individual container can have its own IP address and its own network configuration. You can also go ahead and change those network configurations.

Setting up a Linux server, you need a Linus operating system such as Ubuntu. Then you install the Linux Container software. once installed, you can log into it, and you will get a prompt that looks like any other Linux client

### Docker containers

Docker is a containerized virtual environment that makes it easy to develop, maintain, and deploy applications and services. Docker containers are incredibly light, and you don't have to configure or set up virtual machines and environments, meaning you have hypervisors in between. Docker containers work off directly from the host OS. Essentially it doesn't have a separate kernel to run its containers. It utilizes the same resources as the host OS. Docker exploits namespaces and control groups to let you use host OS resources more efficiently.

Let's interpret this with a diagram.

![A Docker container](/engineering-education/lxc-vs-docker-what-is-the-difference-and-why-docker-is-better/a-docker-container.png)

[Image source](https://www.Docker.com/resources/what-container)

The above containerization concept has the infrastructure and an operating system added to it. In contrast to LXC, there is no guest operating system. Instead, it has a Docker demon that runs directly on the operating system. Docker demons facilitate creating, running, and managing the containers. Each instance of a container runs one process (application). Each application is isolated and runs without affecting other applications. So, in this case, each app is containerized with its own configuration files.

Docker demons allow containers to pingback resources from the host machine. In this case, the demons allocate resources to this container depending on how much a container needs to run.

### What is the major difference between Linux and Docker containers

LXC focuses on OS containerization, while Docker thrives on application containerization. Docker is single-purpose application virtualization, and LXC is multi-purpose operating system virtualization.

In this case, LXC specializes in deploying Linux Virtual machines. A container is like a VM with a fully functional OS environment. However, the OS has to support and handle the features and capabilities of a Linux environment. You can SSH into an LXC container, operate it like an operating system, and install any application or services, and everything will work as expected.

This is not the case with Docker. Docker specializes in deploying applications. Docker containers aren't lightweight virtual machines. Thus they can't be considered as such. Docker containers are limited to a single application due to their architecture. Although Docker runs natively in a Linux environment, it is not entirely dependent on Linux, and it supports other operating systems such as Windows and macOS.

### Why Docker is better

#### Portability

Probably you have heard a phrase it works on my computer but not theirs.

Consider a case where a company hires a developer to develop an app on oracle WebLogic software. This means the developer has Oracle WebLogic installed on the computer. When the application is fully developed, the developer will share this application with the team, testing the code. The team will then have to repeat the process of installing and configuring the Oracle WebLogic. The same application is shared with the production team, and the cycle repeats itself.

Due to the environmental difference on these three levels, the installation was done separately. This is one of the most comprehensive solutions that Docker container offers. Docker will containerize this single application with all the configuration the application needs to run. The other team only needs to have the Docker demons installed. Then the developer can share this application without having to install additional software regardless of the operating system the other teams are in.

Docker goes hand in hand with versioning. When sharing this application, the version of the services used doesn't change. Thus, the whole application will be shared without breaking the code.

### Flexibility

As we have explained above, Docker knows no environmental boundaries. Linux containers only support containerization with a Linux-based operating system. Thus a container only runs in one environment. That's not the case with Docker. If it works on your computer, it will work on my computer as well. Docker engine is well set to work constantly across all environments. If you set an application on a Windows-based server, it can be easily deployed in a Linux server without compatibility issues.

With Docker containers, applications will work efficiently and correctly in different computer environments. it can be easily deployed and executed on other computer environments regardless of their host operating system or configurations

#### Efficient use of system resources

Docker containers have no kernels of their own or virtual memory and CPU, making it easier to utilize resources directly from the host infrastructure. There is no added resource allocation complexity. Resources are served as demanded. An application only uses resources if up and running. And if it's running, it is the role of the Docker engine to assign resources to this application directly from the physical resources of the host. You would be able to pull off many more Docker containers than the Linux containers in a single host OS.

Also, the Docker engine allows you to run multiple containers while sharing resources and still maintain isolation. In Docker, an application is specified using a Docker image. Docker allows you to build or stack an application on top of previously created packages, which provides a great possibility for component usability. Assume you need to create many instances, each of which requires Apache and MySQL. In such a situation, you can create a "base image" that includes these two pieces, then construct and generate more instances that already have them installed.

#### Lightweight thus fast

You don't have to set up a hypervisor or configure virtual machine images that are hard to set up and deploy. And there's no need to put up a guest OS.  Docker is quick as a result of this. To get to your application, you don't have to start up a complete operating system.

Docker engine has embedded configuration management commands that let you start to stop pr restart a container within seconds right there in your host machine. Docker containers take up less disk space with no complex configuration to set, making them relatively faster in executing applications.

#### Popularity

If popularity would be the only factor to be considered when choosing between these two containerization solutions, Docker would easily defeat LXC. Docker was launched in 2013. in its early stages, Docker used LXC but has subsequently modified its codebase to create a completely new container architecture.

Many IT titans, including Netflix, Twitter, Google, and other web-scale organizations, have embraced Docker's application containerization strategy for its scalability benefits. Docker's popularity is booming, [according to ZDNet]((https://www.zdnet.com/article/what-is-Docker-and-why-is-it-so-darn-popular/), with over 3.5 million container-based applications and billions of container orchestration distributed using Docker.

One of the reasons for Docker's popularity is the approach it used to concur its target market. Containers were designed to go beyond the LXC Operating system and into the more detailed realm of an application that fits many organizations and enterprises.

### Conclusion

LXC offers the advantages of a VE on Linux, most notably the ability to isolate your Linux OS from one another. It's a cheaper and faster way to build compared to a VM, but it demands additional Linux knowledge and experience.

On the other hand, LXC's capabilities have been significantly enhanced by Docker. Docker is garnering a rising user base due to its evident benefits and capacity to share and duplicate any Docker-created packages. It is taking the lead above VMs and VEs.

As a developer is good to learn these technologies which are taking the IT world by storm. Other containerization technologies are worth checking include [Kubernetes](https://kubernetes.io/), [Amazon ECS](https://aws.amazon.com/ecs/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&ecs-blogs.sort-by=item.additionalFields.createdDate&ecs-blogs.sort-order=desc), [rkt](https://www.openshift.com/learn/topics/rkt), and [Mesos](http://mesos.apache.org/)