# jenkins

We are going to place the learning of our Jenkins starting from basics to advanced in this repository



# Backlog 

```
    Configure a webHook between Jenkins and GitHub.
    So that when there is any commit on xyz-repo, jenins-xyx would be triggered automatically.
```


Imp : Stages on jenkins are sequential, that means stage 2 will only run after the completion of stage-1 


Jenkins is all about plugins ?
A plug-in is nothing but a feature, if you want a feature, install that plugin.



### What if one of the JOB that you're running on jenkins go crazy and run the the for indefintiely & exhausts the resources ?
    > Jenkins will recounter out of memory or outOfResources error and untimately jenkins will be down which is a potential loss to teams.


### What all could be the reasons on why a machine/server can go down ?

    *   Out Of Memory / CPU / Disk ( resources )
    *   Hardware Failure ( Chip / board / cpu failure / disk failure )
    *   Kernal Panic 

```A kernel panic is a critical system error that halts all operations on a Linux system. It occurs when the Linux kernel, the core of the operating system, encounters an unrecoverable issue and can no longer function safely. Kernel panics can be caused by software bugs or hardware malfunctions.```

#### Common Causes Of Kernal Panic :

```
    Corrupted system files (e.g., initramfs)
    Faulty hardware (e.g., RAM, overheating)
    Incompatible kernel modules
```

### Jenkins-Mastre-Agent Architecture 

    What are the pre-requisties that you need to consider for a node before you add that as a node to as Jenkins Controller ?

        *   Ensure you install JAVA on the top of the NODE ( because Jenkins is a java based product )
        *   Jenkins authenticates using ssh, so make sure you node and jenkins can communicate with each other 
        *   Ensure the security of the NODE Machine has 22 open for the Jenkins Controller ( Jenkins should be able to ssh to the agent machine )


### How can we control the number of jobs that runs on a specfic agent ?
    * We need to define that option while configuring the agent and we refer that as "Number Of Executors"


### If you have 24 nodes that are added to your Jenkins-Controller and you don't have access to them ? How can you see the disk utilization or the space availability on the top of them ?

> You can see that information on the top of Manage Jenkins ---> Nodes ( it shows you the available free space )
