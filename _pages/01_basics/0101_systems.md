---
layout: default
title: Systems
permalink: /systems/
tags: ['basics']
---

# Systems

An xGraph **system** is described as a collection of one or more modules that are instantiated by a single 
kernel process (Nexus.js in node). This means that an xGraph system is simply a collection of xGraph modules 
that are run as a single process using the xGraph core functionality.

A **system definition** includes a **system structure** and the **system cache**. For information on how to 
get started building xGraph systems, see [Generating Systems](/generating-systems/).

The **system structure** is a JSON object that describes the way a system should be built. The system 
structure object has two parts: `"Sources"`, and `"Modules"`.

xGraph systems are easy to compile and run using the [Getting Started](/getting-started/).

Here is an example of a simple system structure.

```
{
    "Sources": {
        "core": "{core}"
    },
    "Modules": {
        "RootView": {
            "Module": "xGraph.RootView",
            "Source": "core",
            "Par": {
                "Layout": "$3DView"
            }
        },
        "3DView": {
            "Module":"xGraph.3DView",
            "Source": "core"
        },
        "Deferred": [
            {
                "Module": "xGraph.Mouse",
                "Source": "core"
            },
            {
                "Module": "xGraph.Popup",
                "Source": "core"
            },
            {
                "Module": "xGraph.Modelx3D",
                "Source": "core"
            }
        ]
    }
}
```
The first section, the `"Sources"` key of the JSON object, variable paths for module sources. This is where 
xGraph will look for the modules. These can be module brokers, places that serve modules, or a path to a 
local file space where modules are being developed. 

Typically, as in this example, you will want to use a command line parameter to define the exact location of 
modules. You denote a variable that will be replaced using a command line parameter by surrounding it with 
curly brackets, as seen in our example:
```
    "Sources": {
        "core": "{core}"
    }
```
Here, the text `{core}` will replaced with the string given with the `core` flag when the system is run. 
Using the xGraph CLI, this would look like the following command.
```
xgraph reset --cwd .\Systems\3DViewExample\ --core .\Modules\
```

The next section, the `"Modules"` key, has information about all the modules in the system. First, you will 
see modules that are explicitly defined, using their module definition. These will be loaded when the system 
starts up. Details about module definitions can be found in the [Module Guide](/modules/). 

Next inside of the `"Modules"` key is the `"Deferred"` array. This array is a list of modules the system 
needs to run that are not loaded when the system starts up. Typically generated by other modules, the 
system's `"Deferred"` modules will be loaded into a system's cache, so the system does not need to access 
external sources when it is run.

A **system cache** has everything you need to run an xGraph system. That means you can develop a system on 
any machine, compile the cache, and run it from the cache on another machine. It also means xGraph doesn't 
need any external dependencies to run a system, so the system can be run on a machine that does not have an 
internet connection. 
