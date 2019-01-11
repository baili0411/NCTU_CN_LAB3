# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution

> TODO:
> * How to run your program?
> * What is the meaning of the executing command (both Mininet and Ryu controller)?
> * Show the screenshot of using iPerf command in Mininet (both `SimpleController.py` and `controller.py`)

---
## Description

### Tasks

1. Environment Setup
    * Step 1: Join this lab on GitHub Classroom and get initial repository
    * Step 1.5: Clone repository outside of container, in own computer.  
    Do this for easier editing of README.md.
        ```
        git clone https://github.com/nctucn/lab3-baili0411.git
        Move into folder
        git checkout -b Readme
        ```
    * Step 2: Login in to container using SSH.  
    Access the container. Login as root. Didn't change the password.
    * Step 3: Clone your Github repository and setup Git in the container.  
    First, setup global username and email  
        ```
        git config --global user.name "Baili Deng"
        git config --global user.email "rayman0411@gmail.com"
        ```  
        Then clone Github repository.
        ```
        git clone https://github.com/nctucn/lab3-baili0411.git Route_Configuration
        ```
    * Step 4. Run Mininet for testing
        ```
        sudo mn
        mn
        ```
        ![Results](mn_result.png)
        ```
        sudo service oepenvswitch-switch start
        mn
        ```
        ![Results](service_result.png)

2. Example of Ryu SDN
    * Step 1. Login to your container in two terminals
    Used tmux and create a new panel.
        ```
        tmux
        ```
    * Step 2. Run Mininet topology  
    Run SimpleTopo.py in one panel.  
        ```
        cd Route_Configuration/src
        mn --custom SimpleTopo.py --topo topo --link tc --controller remote
        ```
        Result after running SimpleTopo.py  
        --custom means we run mn, and invoke the "topo" topology constructor, the "tc" link constructor, and the "remote" controller constructor in SimpleTopo.py
    ![Results](mininet_sample_result.png)
    * Step 3. Run SimpleController.py in another terminal
    Switch the pane.
    Run SimpleController.py
        ```
        cd Route_Configuration/src
        ryu-manager SimpleController.py --observe-links
        ```
        ryu-mangager --observerlinks means that we observe link discovery events.  
        Result after running SimpleController.py
        ![Results](ryu_manager_sample_result.png)
    * Step 3. Leave Ryu controller  
    Leave mininet first.
        ```
        mininet>exit
        ```
        Switch Panel over to Ryu controller, and stop it with Ctrl-z.
        Switch back to mininet panel, and clean links.
        ```
        mn -c
        ```
3. Mininet Topology
    * Step 0. Create a new branch for adding the topology.
    Create a new branch and move to it.
        ```
        git checkout -b topology
        ```
    * Step 1. Build the topology via Mininet
    Build the topology based on topo.
    ![Topology image](src/topo/topo.jpg)
    Copy a topo.py based on SimpleTopo.py
        ```
        cp SimpleTopo.py topo.py
        ```
        After checking SimpleTopo.py, the parts we need to add are:  
        * Although there isn't a controller c0 added in the SimpleTopo.py, based on the result of executing SimpleTopo.py with mininet, there is a controller c0, so no need to add that.
        * Bandwidth, delay loss on links s1-s3, s1 -s2, s2-s3
        ```
        self.addLink(s1, s3, port1=3, port2=2, bw = 3, delay = '10ms', loss = 3)          
        self.addLink(s1, s2, port1=2, port2=1, bw = 30, delay = '2ms', loss = 1)          
        self.addLink(s3, s2, port1=3, port2=2, bw = 20, delay = '2ms', loss = 1) 
        ```
4. Ryu Controller

5. Measurement

### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.
   
2. What is “table-miss” in SDN?
   
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?
   
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, CONFIG_DISPATCHER)
    ```

5. What is the meaning of “datapath” in `controller.py`?
   
6. Why need to set "`ip_proto=17`" in the flow entry?
   
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.
   
8. Which forwarding rule is better? Why?

---
## References

* **Ryu SDN**
    * [Ryu Documentation](https://ryu.readthedocs.io/en/latest/man/ryu_manager.html?highlight=--observe)
* **Mininet**
    * [Introduction to Mininet](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Git Setup Guide](https://git-scm.com/book/zh-tw/v2/%E9%96%8B%E5%A7%8B-%E5%88%9D%E6%AC%A1%E8%A8%AD%E5%AE%9A-Git)
    * [Tmux shortcuts and cheatsheat](https://gist.github.com/MohamedAlaa/2961058)

---
## Contributors

* [Baili Deng](https://github.com/baili0411)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3