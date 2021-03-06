#RepEX: Replica Exchange simulations Package

This package provides functionality to run simple synchronous Replica Exchange (temperature exchange) simulations on local system. Currectly RepEX uses NAMD as it's application kernel.   

###Theory of Replica Exchange simulations

In Parallel Tempering (Replica Exchange) simulations N replicas of the original system are used to model phenomenon of interest. Typically, each replica can be treated as an independent system and would be initialised at a different temperature. While systems with high temperatures are very good at  sampling large portions of phase space, low temperature systems often become trapped in local energy minima during the simulation. Replica Exchange method is very effective in addressing this issue and generally demonstrates a very good sampling. In RE simulations, system replicas of both higher and lower temperature sub-sets are present. During the simulation they exchange full configurations at different temperatures, allowing lower temperature systems to sample a representative portion of phase space.


##Installation instructions

```bash
$ virtualenv $HOME/myenv 
$ source $HOME/myenv/bin/activate 
$ git clone https://github.com/radical-cybertools/ReplicaExchange.git 
$ cd ReplicaExchange
$ python setup.py install
```

Then you can verify that Radical Pilot was installed correctly:
```bash
$ radicalpilot-version
```

This should print Radical Pilot version in terminal
 
##Usage

###Local simulation example

First you need to copy a block of input parameters required for executing simulation on local system from /re_package/config/config.info to /re_package/config/input.json. Further details on how to do this are provided in /re_package/config/config.info file. Next, you need to specify path to NAMD executable, details of how this should be done are provided in /re_package/config/config.info file. In this file you can find NAMD paths for mac and linux.

To run RE simulation on your laptop/desktop, you need to specify simulation configuration file and resource configuration file. In order to run provided RE example run the following commands in terminal: 

```bash
$ cd re_module
$ RADICAL_PILOT_VERBOSE=info python radical_re_namd.py --input='config/input.json' --resource='config/xsede.json'
```

This will run RE temperature exchange simulation involving 6 replicas on your local system. Simulation will perform a total of 3 cycles and after each cycle temperatures of replicas will be exchanged. During the simulation input files for each of the replicas will be generated. After simulation is done in ReplicaExchange/re_package directory you should see a number of NAMD output files for each replica, namely coordinate, velocity and extended system files. 

###Remote (Stampede) simulation example

In order to run this example you don't need to login to Stampede. All you have to do is to launch the simulation as described below. 
As for previous exemple you first need to copy a block of input parameters required for executing simulation on Stampede from /re_package/config/config.info to /re_package/config/input.json. Further details on how to do this are provided in /re_package/config/config.info file.

To run RE simulation, you need to specify simulation configuration file and resource configuration file. In order to run RE simulation remotely on Stampede execute the following commands in terminal: 

```bash
$ cd re_module
$ RADICAL_PILOT_VERBOSE=info python radical_re_namd.py --input='config/input.json' --resource='config/xsede.json'
```

This will run RE temperature exchange simulation involving 12 replicas on Stampede remotely. Simulation will perform a total of 3 cycles and after each cycle temperatures of replicas will be exchanged. During the simulation input files for each of the replicas will be transferred to your local system. After simulation is done in ReplicaExchange/re_package directory you should see a number of NAMD output files for each replica, namely coordinate, velocity and extended system files. 

###input.json 

**input.PILOT**

In this part of input file must be specified Pilot releted paramers. 

- resource: name of the system to use, this should correspond to resource name defined in resource configuration file, e.g. xsede.json  

- sandbox: working directory on the target system

- cores: number of cores the pilot should allocate on the target resource 

- runtime: estimated total run time (wall-clock time) in minutes of the ComputePilot

- mongo_url: url to MongoDB database

- cleanup: boolean variable to specify if ComputeUnit and pilot directories should be deleted or not  

**input.NAMD**

In this part of json input file must be specified NAMD releted paramers. 

- namd_path: path to NAMD executable on target system

- input_file_basename: base name of NAMD input file (.namd); this file is used to create individual input files for replicas 

- namd_structure: path to namd structure file (.psf)

- namd_coordinates: path to namd coordinates file (.pdb)

- namd_parameters: path to namd parameters file (.params) 

- number_of_replicas: number of replicas to be launched on a target system

- min_temperature: minimum temperature for replicas

- max_temperature: maximum temperature for replicas  

- steps_per_cycle: number of steps each replica performs in one cycle

- number_of_cycles: number of cycles to perform during simulation

All other parameters must be specified in NAMD input file directly!

###xsede.json 

In this file must be specified resource details for the target machine. Please check Radical Pilot documenation for further details: http://radicalpilot.readthedocs.org/en/latest/machconf.html 



