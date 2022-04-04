# Good practices

---

## Git repository handling

---

### Introduction to Git

[Git](https://git-scm.com) is a free and open source distributed version control system designed to handle projects sources with speed and efficiency. The first step is to get through the basic theory of git. While a good mastery of git requires several hours of study and practices, basic use of the elementary notions **commit**, **push**, **pull** and **branches** can be mastered in a reasonable amount of time. Plenty of tutorials are availables on that matter.

### Repository hosting

As for the tutorials, numerous hosting platforms for git repository are availables GitLab, BitBucket or GitHub. The latter being the one currently used for many reasons amongs its convenient organization features.

I advise you to get a clear Name/Username for your GitHub profile to make it simple for organization management.

```Bash
If your Bruce Wayne a good profile would be
Name : Bruce Wayne
Username : bwayne
Bio : Intern at Gotham University
```

### Environment

Depending on your skills and experience you could use a full command approach with bash or rely on a graphic interface such as GitHub Desktop or Sourcetree. Sourcetree being the one currently used for its user-friendly approach and its incorporated bash.

### Organize folder

Here is the organization I typically use for a git repositories that targets FPGA implementation and usually implies several languages. Any organisation is fine as long as it has been thought through, well-documented and well-organised. A good practice consists in placing .gitignore in projects folder to keep the tree structure of the repo.

```Bash
Git-Repo
├── README.md
├── c
├── data
├── docs
├── fpga
│   ├── pl
│   │   ├── hdl
│   │   ├── py
│   │   ├── tb
│   │   ├── vitis_project
│   │   ├── vivado_project
│   │   │   ├── cmod
│   │   │   └── genesys2
│   │   ├── waves
│   │   └── xdc
│   └── zynq
│       ├── cpp
│       ├── hdl
│       ├── py
│       ├── tb
│       ├── vitis_project
│       ├── vivado_project
│       │   ├── zcu102
│       │   └── zybo
│       ├── waves
│       └── xdc
├── LICENSE
└── matlab
    └── functions
```

### Create branches

Even if you are mostly working alone on repository, the use of branches is still recommended. A simple branch structure is a master branch and a development branch. The master branch stages all working changes so as it is stable and functionnal. The development branch stages your current progress on the project. You can either create a branch for each change you make or feature you make, then merge and delete when it's good to go. In that case you might name your branch with a recurring pattern like wip001-add-parameter-sweep. Another option is to keep a single development branch all along the project and merge at points where it is functionnal and stable. I usually go with a branch called dev followed by my initials.

### Push daily

Staging your changes online once a day at the end of the day is a good practice to have. It will first prevent data losses that may occur (whims of windows, earthquake, theft, bicycle accident, ... _it might seems quite paranoid but it does happen_). Your collaborators from different time zones as well as supervisors will be able to follow up your progress regularly facilitating meetings and advicing. Moreover, you have access to your up to date sources almost everywhere.

### License your repository

Github allows you to easily associate your repository with a license on creation. License states the intellectual property and provides a clear undestanding of how the user can arrange your work.

### Staging large files

Avoid staging large files since GitHub repositories have a 100 MB limit. Vivado/vitis projects have to be local but they can be exported as tcl scripts (prodecure detailed [here](#tcl-scripts-for_vivado_projects)). If staging large files is really required you should consider using **git lfs**.

### Few more rules

* /!\ : Don't stage large files, GitHub repositories have a 100 MB limit (unless you use git lfs)
* /!\ : Always pull before pushing anything
* /!\ : Use only relative path or environment variables
* /!\ : Commit messages in present (fix this, add that, ...)

### Links

[**GitHub**](https://github.com/)

[**Sourcetree**](https://www.sourcetreeapp.com/)

## Documentation

---

### Simple publish Github Pages from documentation repository

Since Github only allows to serve static html from either /root or /docs of a given branch and
Sphinx outputs html under /build/html it is convenient to script documentation generation to
obtain a structure easily publishable using github pages.

One of the many solution is to organize the repository as below :

```bash
Git repo
├───docs
│   └───*.html
└───src
    └───docs
        ├───build
        ├───source
        ├───Makefile
        └───make.bat
```

* **src/docs** is your local Sphinx folder with sources and build

* **docs** is html static content for github pages

Then the Makefile is modified to generate Sphinx documentation and copy its output in docs folder.

```Makefile
# Folder for html output
DOCS          = ../../docs

# Add new make file directive : make github
github:
    # Clean build folder
	rm -rf "$(BUILDDIR)"
    # Generate Sphinx documentation
	make html
    # Copy build in github pages source folder being /docs
	cp -r "$(BUILDDIR)/html/." "$(DOCS)"
    # Create .nojekyll file to setup github pages correctly
	echo $ >> "$(DOCS)/.nojekyll"
```

Therefore, you can stick to `make html` to locally check changes in src/ then you may use `make github` and stage/push your changes.

You may also add the local build to the .gitignore.

```py
# Ignore local build for sphinx doc
docs/build/
```

_Better solutions use a different branch staging only static html content updated with Github Actions or git hooks. For the time being this solution is easier to set up and suits our needs._

## C++

---

### Comments

#### Header

Adding a header at the beginning of all yours files. It will help you keeping track on your sources and defusing potential cases of plagiarism.
A header usually includes the name of the authors, the creation and update date and versioning as well as comments.

```C++
/*
*! @title      Example code
*! @file       example.cpp
*! @author     Bruce Wayne, Clark Kent
*! @date       01 Jan 2021
*! @version    0.1
*! @copyright
*! SPDX-FileCopyrightText: © 2018 Bruce Wayne <bwayne@gmail.com>
*! SPDX-FileCopyrightText: © 2021 Clark Kent <ckent@gmail.com>
*! SPDX-License-Identifier: MIT
*! @brief
*! Example code
*! * nothing much to say
*! 
*! @details
*! > **01 Jan 2018** : file creation (BW)
*! > **01 Jan 2021** : fix module import (CK)
*\
```

#### Functions

Here is an example for function comments.

```C++
/***************************************************************************
 * Compute Ohm's law
 *
 * u = r * i
 * 
 * @param r Resistance - ohm
 * @param i Current - A
 * @return Voltage - V
****************************************************************************/
int calcVoltage(float r, float i);
```

## Matlab

---

### Comments

Comments also are essential. There are plenty of way to handle it since Matlab is widely use by people from non-programming background. Here are some rules I usually apply for my scripts.

#### Header

Adding a header at the beginning of all yours files. It will help you keeping track on your sources and defusing potential cases of plagiarism.
A header usually includes the name of the authors, the creation and update date and versioning as well as comments.

```Matlab
% @title      Example script
% @file       example.m
% @author     Bruce Wayne, Clark Kent
% @date       01 Jan 2021
% @version    0.1
% @copyright
% SPDX-FileCopyrightText: © 2018 Bruce Wayne <bwayne@gmail.com>
% SPDX-FileCopyrightText: © 2021 Clark Kent <ckent@gmail.com>
% SPDX-License-Identifier: MIT
%
% @brief
% Example script
% * nothing much to say
% 
% @details
% > **01 Jan 2018** : file creation (BW)
% > **01 Jan 2021** : add init from file (CK)
```

#### Functions

Insert comments below functions is a standard, the | and ** are syntax used by Sphinx for documentation layout.

```Matlab
function u = ohmLaw(r, i)
% | **Compute ohm's law**
% |
% | **u** : Voltage (V)
% | **r** : Resistance (ohm)
% | **i** : Current (A)
%
% | Compute simple ohm's law from fixed parameters in SI unit
```

#### Sections

Sections (comments blocks that start with %%) are usually followed by a consistent name.
They can be divided in blocks separated by simple line comment with a simple tab.

```Matlab
%% Define Parameters ##########################################################################
    % Time
        ...

    % Geometry
        ...
```

#### Inline

Inline comments are mostly used for variables declaration or assignation so as they describe what the variable is representing, what operation is performed or what is the unit.

```Matlab
    dt              = 2^-5;     % ms - Time step
    sim_time        = 30e3;     % ms - Simulation time

    neur_diam       = 15;       % µm - Average diameter of a neuron
    dist_orgs       = 120;      % µm - Distance between centers of organoids
```

## Python

---

### Comments

#### Header

As for matlab, adding a header will help you keeping track on your sources and defusing potential cases of plagiarism.
As explained before, header usually includes the name of the authors, the creation and update date and versioning as well as comments.

```Python
# -*- coding: utf-8 -*-
# @title      Example script
# @file       example.py
# @author     Bruce Wayne, Clark Kent
# @date       01 Jan 2021
# @version    0.1
# @copyright
# SPDX-FileCopyrightText: © 2018 Bruce Wayne <bwayne@gmail.com>
# SPDX-FileCopyrightText: © 2021 Clark Kent <ckent@gmail.com>
# SPDX-License-Identifier: MIT
#
# @brief
# Example script
# * nothing much to say
# 
# @details
# > **01 Jan 2018** : file creation (BW)
# > **01 Jan 2021** : fix module import (CK)
```

#### Variables

Variables can be commented as below

```Python
#: UART communication speed
BAUD_RATE = 115200

CLOCK_SPEED = 115200 #: Main system clock speed - MHz
```

#### Functions

Functions can be commented as below.

```Python
def sum(a, b):
    """Sum a and b

    :param int a: Number to sum
    :param int b: Number to sum

    :returns: the sum of a and b
    :rtype: int
    """
    return a+b
```

#### Class

Here is an example for a class.

```Python
class Bird:

    """Describes a simple Bird

    :param str name: Bird's name.
    :param str color: Bird's color.
    """

    def __init__(self, name, color):
        """Constructor"""
        self.name   = name
        self.color = color

    def fly(self, z, duration):
        """ 

        .. WARNING::

            Be careful don't fly to high or your wings might melt

        :param double z: Flying altitude - meters
        :param int duration: Flying duration - seconds
        """
        pass
```

## VHDL

---

### Comments

VHDL is known to be a verbose language, meaning that it requires a lot of detailed syntax. Therefore, wisely and efficiently commenting is highly recommanded both for your own mental health and for code sharing. The following examples describes docstring oriented commenting practices that can be interpreted either by Doxygen or TerosHDL ([TerosHDL documenter](https://terostechnology.github.io/terosHDLdoc/features/documenter.html)).

#### Header

A documentation-friendly approach based on Doxygen docstrings and that works well with TerosHDL is as below.

```vhdl
--! @title      Single port RAM
--! @file       SPRAM.vhd
--! @author     Bruce Wayne, Clark Kent
--! @date       01 Jan 2021
--! @version    0.1
--! @copyright
--! SPDX-FileCopyrightText: © 2018 Bruce Wayne <bwayne@gmail.com>
--! SPDX-FileCopyrightText: © 2021 Clark Kent <ckent@gmail.com>
--! SPDX-License-Identifier: GPL-3.0-or-later
--!
--! @brief
--! Single port RAM with generic RAM and DATA size
--! * can be initialized from a file
--! 
--! @details
--! > **01 Jan 2018** : file creation (BW)
--! > **01 Jan 2021** : add init from file (CK)
```

_Better copyright and licensing claiming that complies to current standards will be uploaded soon but for now they are stated following the format below ([source](https://matija.suklje.name/how-and-why-to-properly-write-copyright-statements-in-your-code))._

```Bash
SPDX-FileCopyrightText: © {$year_of_file_creation} {$name_of_copyright_holder} <{$contact}>
SPDX-License-Identifier: {$SPDX_license_name}
```

Header can be completed by a [waveform description](https://wavedrom.com/tutorial.html) of the module behavior if you are using TerosHDL ([TerosHDL documenter](https://terostechnology.github.io/terosHDLdoc/features/documenter.html)).

#### Ports

It is sometimes useful to organise properly the inputs and outputs of a module.
However, it may sometimes hamper column selection and incremental selection as your
lines are getting sparsed. Here is a quick example. The "!" after the comment marker is used to generate doctrings.

```vhdl
port(
    -- Clock/reset --
    clk     : in  std_logic;    --! Clock
    reset   : in  std_logic;    --! Reset

    -- SPI --
    DAC_sclk    : out std_logic;    --! System clock
    DAC_cs      : out std_logic;    --! Chip select
    DAC_mosi    : out std_logic;    --! Master output slave input

    -- PSU -> FPGA via AXI LITE
        -- RAM control
        PSU_en_write    : in std_logic; --! RAM write enable from PSU via AXI
        PSU_addrw       : in std_logic_vector(ADDRSIZE-1 downto 0); --! RAM write address from PSU via AXI

        -- Conductance
        PSU_GK      : in std_logic_vector(INT+DEC downto 0) --! Conductance from PSU via AXI
);
```

#### Signal declaration

About the same principles apply to signal declaration. Try to regroup your signals according to their role or to the modules they are connecting to. Here is another example.

```vhdl
    -- Clock --
    signal clk              : std_logic; --! Main clock

    -- DSP pipelines
    signal A    : signed(7+DEC downto 0)    := (others => '0'); --! DSP multiplier inputs
    signal B    : signed(7+DEC downto 0)    := (others => '0'); --! DSP multiplier inputs

    -- Time --
    signal tick             : std_logic := '0';                 --! Computation enable
    signal tick_counter     : std_logic_vector(11 downto 0);    --! Counts clock cycle to generate ticks
```

### Think generic

Another important parameter is the genericity of your design. Using generic parameters instead of fixed parameters will clarify your description and simplify the development.

#### Constants

The use of constants in VHDL is really imporant, it helps generic generation and really clarifies your code. Moreover, they do not relate to any material generation since they correpond to GND or VCC connections on pins (quite similar to the C preprocessor directive #define). Let's take the example of a FPGA implementation to get the signal from 4 parallel ADC coded on 12 bits, a wise description could start with the following description.

```vhdl
    constant NB_ADC     : integer range 0 to 4      := 4;       -- Number of ADC to read
    constant NB_BIT_ADC : integer range 0 to 12     := 12;      -- Number of bits for ADC signal encoding
    type data_ADC_s is array (0 to NB_ADC-1) of std_logic_vector(NB_BIT_ADC-1 downto 0);    -- Signal type for an array of ADC signals
    signal data_ADC     : data_ADC_s                := (others=>(others=>'0')); -- Signal for all ADC data

    -- data_ADC(0) is a 12 bits vector corresponding to the data of your first ADC
```

#### Make packages

As well defined on [nandland](https://www.nandland.com/vhdl/examples/example-package.html), a package in VHDL is a collection of functions, procedures, shared variables, constants, files, aliases, types, subtypes, attributes and components. It can, in some extents, roughly be seen as a library in C. Using packages will allow global signal type and constants declaration as long as you include the package in the module. A good example of package is also given on [nandland](https://www.nandland.com/vhdl/examples/example-package.html).

* Package file description

```vhdl
-- Package Declaration Section
package example_package is
 
  constant c_PIXELS : integer := 65536;
 
  type t_FROM_FIFO is record
    wr_full  : std_logic;
    rd_empty : std_logic;
  end record t_FROM_FIFO;  
   
  component example_component is
    port (
      i_data  : in  std_logic;
      o_rsult : out std_logic); 
  end component example_component;
 
  function Bitwise_AND (
    i_vector : in std_logic_vector(3 downto 0))
    return std_logic;
   
end package example_package;
 
-- Package Body Section
package body example_package is
 
  function Bitwise_AND (
    i_vector : in std_logic_vector(3 downto 0)
    )
    return std_logic is
  begin
    return (i_vector (0) and i_vector (1) and i_vector (2) and i_vector (3));
  end;
 
end package body example_package;
```

* Using package in modules

```vhdl
library work;
use work.example_package.all;
```

#### Smart signals assignation using 'length

Another step toward generic design comes with the use of generic signal assignation. A very useful property of VHDL signals is the reference to their length that helps signal assignation and concatenation. Here are some applications.

* Assign a value using build-in functions

```vhdl
signal counter : unsigned(31 downto 0); -- constants works well in that case too
counter <= to_unsigned(15, counter'length);
```

* Assign from a larger signal to smaller one (truncate)

```vhdl
-- let's assume we want to get a value from the axi registers that are 32 bits wide
signal axi_reg  : unsigned(31 downto 0);
signal value    : unsigned(15 downto 0);
value           <= axi_reg(value'length-1 downto 0);  
-- obviously valid only if your value stored in axi register is intended to be coded on 16 bits 
```

* Assign from a smaller signal to larger one (resize)

```vhdl
-- let's assume we want to write a 16 bits wide value in the 32 bits wide axi registers 
signal axi_reg  : unsigned(31 downto 0);
signal value    : unsigned(15 downto 0);
axi_reg         <= resize(value, axi_reg'length); 
-- the same principle applies if the signals' types are different, 
-- you just have to go through some casts to use resize and assign
```

The use of the **(others => )** syntax also is very useful so as you assign values to a whole signal without having to specify any size.

```vhdl
type t_tab_slv  is array (0 to 5) of std_logic_vector(7 downto 0);   -- Table of slv
signal tab_slv  : t_tab_slv := (others=>(others=>'0'));              -- Init whole table to 0
tab_slv(0)      <= (others => '1');  -- All bits of first vector in table to 1 
```

### Tcl scripts for Vivado projects

#### Export Vivado project in tcl scripts

First, export vivado project:

* Export project as tcl : **File > Project > Write Tcl**
* Untick all boxes
* Set output file path to : **<git-repo\>/fpga/<version\>/vivado_project/**
* Press **OK**

Then, export your block design:

* Export block design as tcl : **File > Export > Export Block Design**
* Set output file path to : **<git-repo\>/fpga/<version\>/vivado_project/**

*Be careful to have a block design name different from the project name. A common practice is to start your block design name by bd_.*

#### Import Vivado project from tcl scripts

First create the project vivado
* Launch Vivado
* Set current path in tcl console to your vivado project path

```Bash
# in Tcl console
cd <git-repo>/fpga/<version>/vivado_project/
```

* Run vivado project tcl script:

```Bash
# in Tcl console
source <your_vivado_project>.tcl
```

* Run block tcl script :

```Bash
# in Tcl console
source <bd_your_vivado_project>.tcl
```

* Create HDL wrapper: **Right click on block design in sources > Create HDL Wrapper**
* Set wrapper as top: **Right click on wrapper in sources > Set as top**

#### Compatible versions

The procedure has been tested under the following environments:
| OS           | Vivado Version   | Status              |
|:-------------|:----------------:|:-------------------:|
| Linux        | 2021.1           | &#10004;            |
| Windows      | 2021.2           | &#10004;            |
