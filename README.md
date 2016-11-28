# Cyber Range Instantiation System (CyRIS)

CyRIS is an open-source tool for automatically creating and managing environments for cyber security
training courses. It is written in python language, and consists of a number of features, including
system configuration, tool installation, incident emulation, content management, and clone
management. For more details, please refer to [1].

## Getting Started

Below are instructions about how to deploy and run CyRIS to on your physical machine.

### Prerequisites

To run CyRIS, your physical machine should have Ubuntu OS installed. We have tested CyRIS running on
Ubuntu Server version 14.04 LTS and 16.04 LTS. In addition, because CyRIS is dedicated for
constructing virtual cyber range environments using KVM virtualization platform, your physical
machine is required to have a processor that supports hardware virtualization. To verify this,
please follow instructions in [KVM/Installation](https://help.ubuntu.com/community/KVM/Installation)
(section "*Check that your CPU supports hardware virtualization*").

It is recommended to have an internet connection in your physical machine, so that CyRIS can connect
to Linux repositories to download and install tools during the cyber range creation process. If your
machine does not have an internet connection, please make sure to have a local repositories so that
CyRIS can refer to.

### Installations

Once you have made sure that your machine meets all the requirements above, please follow steps here
for installing CyRIS.

##### Extract CyRIS source code and base images

After downloading CyRIS source code and sample base images, please extract them somewhere in your
machine (i.e. `/home/crond/cyris-project/cyris` for CyRIS source code, and `/home/crond/cyris-
project/images` for the base images).

##### Install necessary tools on physical machine

In order to be able to use CyRIS, a few tools need to be installed in the physical host. For
automating this task, the bash script `HOST-PREPARE.sh` is given (located under `cyris/` directory).
Please make sure to execute this script succesfully before running CyRIS.

##### Configure CyRIS constants

There are a few constants that need to specify for CyRIS to use. These constants are specified in
the beginning of the source file `cyris/main/cyris.py`, including: 

- ABS_PATH: The absolute path of the directory of CyRIS (i.e. `/home/crond/cyris-project/cyris`).

- GW_MODE: This is for describing the network topology that the physical machine is in. If there
is a gateway machine that stands between the physical machine and the outside network, then the
constant GW_MODE should be set to "True". In contrast, if the physical machine connects
directly to the outside network, the constant GW_MODE will be set to "False".

- GW_ACCOUNT: The user account on the gateway machine.

- GW_MGMT_ADDR: The management address of the gateway machine.

- GW_INSIDE_ADDR: The internal address of the gateway machine.

- USER_EMAIL: The email of the instructor that CyRIS will send detailed information about how to
access the cyber range after finishing the creation process.

Note that if the GW_MODE = False, then the GW_ACCOUNT, GW_MGMT_ADDR, GW_INSIDE_ADDR are not
needed to set.

## Running CyRIS 

Below are steps to run CyRIS.

### Create cyber range description file

The first thing needed to do is describe the desired cyber range environment that you want CyRIS to
create (i.e. how many machines you want in the cyber range, tools and settings on each machine, the
network topology, etc.). The place for doing it is a cyber range description file , which is written
under the YAML format for the readability purpose. A full example of this file is provided in `cyris/CYBER-RANGE-DEF-EXAMPLE.yml`, in that it contains all the details corresponding to each setting.
Please refer to it for more information.

There are also two samples of the cyber range description file (`cyris/main/level1_cyber_range_def.yml` and `cyris/main/level2_cyber_range_def.yml`), which are corresponding to two training scenarios. The first description is about a basic training which includes topics such as log and system configuration review, network sniffing, and vulnerability scanning. The cyber range in this case consists of one machine, which plays the role of a desktop. The base image `images/basevm_desktop_dev` is needed for this scenario. 

The second description is about a training with medium difficulty, in which it includes topics such as network discovery, password cracking, and penetration testing. The cyber range consists of two machines, representing a network of one desktop and one web server. Both the base images `images/basevm_desktop_dev` and `images/basevm_webserver_dev` are required in this scenario.

### Run CyRIS

To run CyRIS, use the command: 

```python *absolute_path*/cyris/main/cyris.py *absolute_path*/*name_of_description_file*.yml```

For example, if you put CyRIS source code under the directory `/home/crond/cyris-project/`, and you want to create a cyber range environment for the second training scenario, then the command should be: 

```python /home/crond/cyris-project/cyris/main/cyris.py /home/crond/cyris-project/cyris/main/level2_cyber_range_def.yml```

### Testing the output

CyRIS records logs of the cyber range creation activities into the file
`/cyris/logs/cr_creation_log`, so it can verify whether or not the creation process is successful by
checking if the word "*error*" appears. However, this error-checking mechanism is insufficient in
many cases, so please take a look at this file yourself if you want to make sure everything is set
up probably.

After CyRIS finishes successfully, there will be an email sent to the `USER_EMAIL` for details about
your cyber range environments, including the summary of how many instances you have in total, and
how to access each one of them.

For example, if you create a cyber range environment for the second training scenario, by following the instructions provided in the email, you will be able to access the desktop machine in your environment.

## Authors

**Pham Duy Cuong, Razvan Beuran**

*Beuran Lab, Information Science, Japan Advanced Institution of Science and Technology*

## References

[1] C. Pham, D. Tang, K. Chinen, R. Beuran, "CyRIS: A Cyber Range Instantiation System for
Facilitating Security Training", International Symposium on Information and Communication Technology
(SoICT 2016), Ho Chi Minh, Vietnam, December 8-9, 2016

