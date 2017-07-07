# <a name="top"></a>File Format Specification NGPF
Version: 0.1-beta  
VISUS, University of Stuttgart  
Oliver Fernandes, Patrick Gralka, Tobias Rau, Guido Reina, Michael Krone

## Sections
[Preface](#preface)  
[General Conventions](#general-conventions)  
[Naming Conventions](#naming-conventions)  
[Supported Binary Codecs](#binary-codecs)  
[Supported Data Types](#data-types)  
[Global Header](#global-header)  
[Time Step Header](#time-step-header)  
[Data](#data)  
[Type File](#type-file)  
[Domain Decomposition](#domain-decomposition)  
[Examples](#examples)

## <a name="preface"></a>Preface 
The NGPF file format focuses on easy *readability of meta data* and *large particle-based* data sets from computational physics, chemistry, biology and more.
The NGPF file format offers human-readable [JSON] headers and a separation of global and per-time-step *parameters*.
Therefore, changes of a *parameter*, such as bounding-box size, can be applied without the need for re-conversion or re-processing of the entire data set.
A user can refer to the header files for a quick overview of relevant *parameters*.
The actual data is stored in a *binary format*.
A user can choose between a variety of *binary codecs* (compression methods) to encode the data (including RAW, i.e. unencoded).
The components of the NGPF file format and their interactions are shown in the schematic below.
A *Global Header* stores the *parameters* relating to all time steps, such as *version number* or the *number of time steps*.
The *Time Step Header* contains the *parameters* of each individual time step (e.g. *number of particles*, *time stamp*).
An optional *Type File* can be used to assign chemical elements to particles, set a per-type color or radius, or describe rigid molecules (no inner DOF) via multiple sites/centers per particle (which should then be interpreted as instances of the rigid molecules).
Additionally, the Type File can contain *parameters* about the element/molecule, such as atomic mass, charge, etc.

<center>
    <img src="schematic.png" ></img><br>
    Figure: Schematic of the file format header management.
</center>


## <a name="general-conventions"></a>General Conventions
[Go to Top](#top)  

In the following we define some terms that will be used throughout this document.
*Parameters* are properties of a data set (or the simulation that generated it) and thus are stored in the header files.
*Attributes* are properties of particles and are stored in the data files.
A *time step* represents a monotonically increasing iteration step of a simulation.
It can be complemented by a real-time *time stamp* relative to the simulation start time.
The term `float` corresponds to an IEEE-754 standard floating point value with a size of 32 bit.
A `byte` is an integer with the size of 8 bit.
The NGPF headers use `JSON` notation with a `utf-8` encoding and a byte order mark (UTF-8 BOM).
The NGPF headers will handle a `string` as a unicode (UTF-8) string.
A string that represents a file path always uses forward slashes `/` to separate directories.
Additionally, all paths are interpreted relative to the directory where the global header is stored.
Paths beginning with `/` are not allowed.
Paths must not contain `../` at all.
The `Version` of the NGPF file format consists of three integer values and a commit hash separated by a dot (`int.int.int.hash`).
The first integer corresponds to the major format version (not necessarily compatible to previous versions).
The second integer is incremented at feature releases (retaining compatibility), and the last integer is incremented at smaller changes and bug fixes.
Integer `int` and float `float` values also support the scientific number format (e.g. `1e2` for `int` and `float` or `1.0e2(.0)` only for `float`).
  
## <a name="naming-conventions"></a>Naming Conventions
[Go to Top](#top)  
The file format supports several pre-defined *attribute* names for convenience:
<table style="width:97%;">
<colgroup>
<col width="25%" />
<col width="75%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Attribute</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>x, y, z</code></td>
<td align="left">Position</td>
</tr>
<tr>
<td align="left"><code>vx, vy, vz</code></td>
<td align="left">Velocity</td>
</tr>
<tr>
<td align="left"><code>r, g, b</code></td>
<td align="left">Color</td>
</tr>
<tr>
<td align="left"><code>rad</code></td>
<td align="left">Radius</td>
</tr>
<tr>
<td align="left"><code>qr, qi, qj, qk</code></td>
<td align="left">Quaternion</td>
</tr>
</tbody>
</table>
All other *attribute* names must comply the following rules:

1. No special characters
2. Use a descriptive string following rule 1 instead of an abbreviation, if the *attribute* name is not common outside your community
3. If a data file contains more than one *attribute* (combination, such as all positional coordinates), you can differ from rule 1 using space (e.g. "x y z")

## <a name="data-types"></a>Supported Data Types
[Go to Top](#top)  
TODO

## <a name="binary-codecs"></a>Supported Binary Codecs
[Go to Top](#top)  
<table style="width:97%;">
<colgroup>
<col width="25%" />
<col width="75%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Raw</td>
<td align="left">Raw storage of data</td>
</tr>
<tr>
<td align="left">ZFP</td>
<td align="left">Floating-point compression scheme <a href="https://github.com/Unidata/compression/tree/master/zfp">ZFP</a></td>
</tr>
<tr>
<td align="left">RLE</td>
<td align="left">Run-length encoding</td>
</tr>
</tbody>
</table>

## <a name="global-header"></a>Global Header
[Go to Top](#top)  
<table style="width:97%;">
<caption> Parameters accepted by the NGPF Global Header.</caption>
<colgroup>
<col width="24%" />
<col width="23%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Format</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>Identifier</code></td>
<td align="left"><code>string</code></td>
<td align="left">Identifies the file as NGPF global header</td>
</tr>
<tr>
<td align="left"><code>Version</code></td>
<td align="left"><code>"int.int.int.hash"</code></td>
<td align="left">NGPF version number</td>
</tr>
<tr>
<td align="left"><code>TimeSteps</code></td>
<td align="left"><code>int</code></td>
<td align="left">Number of time steps</td>
</tr>
<tr>
<td align="left"><code>SimulationTimeUnit</code></td>
<td align="left"><code>string</code></td>
<td align="left">Unit of the <code>SimulationTime</code>, e.g. <code>nanoseconds</code>, <code>custom</code></td>
</tr>
<tr>
<td align="left"><code>MaxSimulationBox</code></td>
<td align="left"><code>[(min) float, (min) float, (min) float, (max) float, ...]</code></td>
<td align="left">Union of simulation boxes over all time steps of the data set</td>
</tr>
<tr>
<td align="left"><code>TypeHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the type header (see <a href="#type-header">Type File</a>)</td>
</tr>
<tr>
<td align="left"><code>TimeStepHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the time step header (see <a href="#time-step-header">Time Step Header</a>) </td>
</tr>
<tr>
<td align="left"><code>TimeStepDirectoryPrefix</code></td>
<td align="left"><code>string</code></td>
<td align="left">Sets the naming scheme for the time step directories (as C format string)</td>
</tr>
<tr>
<td align="left"><code>TimeStepDirectoryIncrement</code></td>
<td align="left"><code>int</code></td>
<td align="left">Defines how many time steps are stored in one directory</td>
</tr>
<tr>
<td align="left"><code>TimeStepAttributeExtension</code></td>
<td align="left"><code>string</code></td>
<td align="left">Defines the file extension of the attribute files inside the time step directories</td>
</tr>
<tr>
<td align="left"><code>TimeStepLayoutColumnName</code></td>
<td align="left"><code>[string, string, ...]</code></td>
<td align="left">Name of each column</td>
</tr>
<tr>
<td align="left"><code>TimeStepLayoutColumnType</code></td>
<td align="left"><code>[string, string, ...]</code></td>
<td align="left">Name of the column type (e.g. <code>float, int</code>)</td>
</tr>
</tbody>
</table>


## <a name="time-step-header"></a>Time Step Header
[Go to Top](#top)  


<table style="width:97%;">
<caption> Parameters accepted by the NGPF Time Step Header.</caption>
<colgroup>
<col width="24%" />
<col width="23%" />
<col width="50%" />
</colgroup>
<thead>
<tr>
<th align="left">Parameter</th>
<th align="left">Format</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>TimeStep</code></td>
<td align="left"><code>int</code></td>
<td align="left">ID of the time step, usually the first time step has id 0</td>
</tr>
<tr>
<td><code>Particles</code></td>
<td align="left"><code>int</code></td>
<td align="left">Number of particles</td>
</tr>
<tr>
<td align="left"><code>SimulationTime</code></td>
<td align="left"><code>float</code></td>
<td align="left">Corresponding time to the time step</td>
</tr>
<tr>
<td align="left"><code>SimulationBox</code></td>
<td align="left"><code>[(min) float, (min) float, (min) float, (max) float, ...]</code></td>
<td align="left">Simulation box of the time step</td>
</tr>
</tr>
<tr>
<td align="left"><code>ParameterOffsets</code></td>
<td align="left"><code>[int, int, ...]</code></td>
<td align="left">Offset inside the data file to the corresponding time step for each attribute.</td>
</tr>
<tr>
<td align="left"><code>Codecs</code></td>
<td align="left"><code>[struct, struct, ...]</code></td>
<td align="left">Defines a struct for each attribute. A struct contains the codec name and the codec properties.</td>
</tr>
</tbody>
</table>


## <a name="data"></a>Data
[Go to Top](#top)  

Each time step range is in a separate directory.
Inside these directories, the data is split into a individual file for each *attribute* or *attribute* combination.
For example in the `x.dat` file, which is located in the `timestep0` directory, the data of *attribute* `x` for time steps `0-9` is stored.
The stored data is compressed by an codec specified by the user.
Additionally, this codec can vary for each time step and each *attribute*.
If more than one *attribute* is stored in a file (e.g. `x y z`), each *attribute* must be stored in separate blocks (i.e. interleaved storage is not allowed).
For combined *attributes*, a specified code applies to all *attributes*.

<table style="width:96%;">
<caption><b>directory for time step 0 to time step 9</b><caption>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<tbody>
<tr>
<th align="left">x.dat</th>
<th align="left">y.dat</th>
<th align="left">z.dat</th>
<th align="left">r.dat</th>
<th align="left">g.dat</th>
<th align="left">b.dat</th>
</tr>
<tr>
<td align="center">ZFP0(x_timestep0)</td>
<td align="center">ZFP0(y_timestep0)</td>
<td align="center">ZFP0(z_timestep0)</td>
<td align="center">RAW0(r_timestep0)</td>
<td align="center">RAW0(g_timestep0)</td>
<td align="center">RAW0(b_timestep0)</td>
</tr>
<tr>
<td align="center">ZFP1(x_timestep1)</td>
<td align="center">ZFP1(y_timestep1)</td>
<td align="center">ZFP1(z_timestep1)</td>
<td align="center">RAW1(r_timestep1)</td>
<td align="center">RAW1(g_timestep1)</td>
<td align="center">RAW1(b_timestep1)</td>
</tr>
<tr>
<td align="center">ZFP2(x_timestep2)</td>
<td align="center">ZFP2(y_timestep2)</td>
<td align="center">ZFP2(z_timestep2)</td>
<td align="center">RAW2(r_timestep2)</td>
<td align="center">RAW2(g_timestep2)</td>
<td align="center">RAW2(b_timestep2)</td>
</tr>
<tr>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
</tr>
</tbody>
</table>

<table style="width:96%;">
<caption><b>directory for time step 10 to time step 19</b><caption>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<tbody>
<tr>
<th align="left">x.dat</th>
<th align="left">y.dat</th>
<th align="left">z.dat</th>
<th align="left">r.dat</th>
<th align="left">g.dat</th>
<th align="left">b.dat</th>
</tr>
<tr>
<td align="center">ZFP10(x_timestep10)</td>
<td align="center">ZFP10(y_timestep10)</td>
<td align="center">ZFP10(z_timestep10)</td>
<td align="center">RAW10(r_timestep10)</td>
<td align="center">RAW10(g_timestep10)</td>
<td align="center">RAW10(b_timestep10)</td>
</tr>
<tr>
<td align="center">ZFP11(x_timestep11)</td>
<td align="center">ZFP11(y_timestep11)</td>
<td align="center">ZFP11(z_timestep11)</td>
<td align="center">RAW11(r_timestep11)</td>
<td align="center">RAW11(g_timestep11)</td>
<td align="center">RAW11(b_timestep11)</td>
</tr>
<tr>
<td align="center">ZFP12(x_timestep12)</td>
<td align="center">ZFP12(y_timestep12)</td>
<td align="center">ZFP12(z_timestep12)</td>
<td align="center">RAW12(r_timestep12)</td>
<td align="center">RAW12(g_timestep12)</td>
<td align="center">RAW12(b_timestep12)</td>
</tr>
<tr>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
<td align="center">...</td>
</tr>
</tbody>
</table>

## <a name="type-file"></a>Type File (optional)
[Go to Top](#top)  
Particles can be identified by a type ID.
The properties of a type are specified in the type file.
NGPF supports a hierarchical structure to reconstruct complex particle combinations or molecules.
However, only the `Name` and the `TypeID` is required to identify particles.
A user can also define *parameters* of any shape to a `TypeID`.

<table style="width:97%;">
<caption> Parameters accepted by the NGPF Type File.</caption>
<colgroup>
<col width="24%" />
<col width="18%" />
<col width="15%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Format</th>
<th align="left">Flag</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>TypeID</code></td>
<td align="left"><code>int</code></td>
<td align="left">Required</td>
<td align="left">Particle type ID</td>
</tr>
<tr>
<td align="left"><code>Name</code></td>
<td align="left"><code>string</code></td>
<td align="left">Required</td>
<td align="left">Name of the particle type (e.g. <code>H</code>)</td>
</tr>
<tr>
<td align="left"><code>NumberSites</code></td>
<td align="left"><code>int</code></td>
<td align="left">Supported</td>
<td align="left">Number of sites of the e.g. molecule</td>
</tr>
<tr>
<td align="left"><code>Color</code></td>
<td align="left"><code>[int, int, int, int]</code></td>
<td align="left">Supported</td>
<td align="left">Color of this type</td>
</tr>
<tr>
<td align="left"><code>Radius</code></td>
<td align="left"><code>float</code></td>
<td align="left">Supported</td>
<td align="left">Radius of this type</td>
</tr>
<tr>
<td align="left"><code>Centers</code></td>
<td align="left"><code>[[x0, y0, z0], [...], ...]</code></td>
<td align="left">Supported, Multisite</td>
<td align="left">Centers of the sites relative to the center of mass</td>
</tr>
<tr>
<td align="left"><code>Types</code></td>
<td align="left"><code>[int, int, ...]</code></td>
<td align="left">Supported, Multisite</td>
<td align="left">Type IDs of the sites</td>
</tr>
<tr>
<td align="left"><code>Quaternions</code></td>
<td align="left"><code>[[qr,qi,qj,qk], [...], ...]</code></td>
<td align="left">Supported, Multisite</td>
<td align="left">Quaternions of connected sites. Can also be empty if a site is, e.g., a single atom.</td>
</tr>
</tbody>
</table>


## <a name="domain-decomposition"></a>Domain Decomposition
[Go to Top](#top)  

In a decomposed simulation, each rank is in charge of one domain.
The most efficient way of writing data is when each rank writes its own data to the storage device.
The optional *Domain Decomposition Header* stores the *parameters* of the domains.
Because every rank writes its own data, each rank also encodes its own piece of the data.
Subsequently, the decoder needs to know the *offsets to each piece* to be able to reconstruct the data as intended.
For this purpose, an additional *offset data file* will be generated for each *attribute*.

<table style="width:97%;">
<caption> Parameters accepted by the NGPF Domain Decomposition Header.</caption>
<colgroup>
<col width="24%" />
<col width="23%" />
<col width="50%" />
</colgroup>
<thead>
<tr>
<th align="left">Parameter</th>
<th align="left">Format</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><code>TimeStepID</code></td>
<td align="left"><code>int</code></td>
<td align="left">ID of the time step</td>
</tr>
<tr>
<td><code>Domains</code></td>
<td align="left"><code>int</code></td>
<td align="left">Number of total domains for this time step</td>
</tr>
<tr>
<td align="left"><code>DDBoxes</code></td>
<td align="left"><code>[[float, ...], [float, ...], ...]</code></td>
<td align="left">Coordinates of the decomposed regions</td>
</tr>
<tr>
<td align="left"><code>DDOffsetExtension</code></td>
<td align="left"><code>string</code></td>
<td align="left">Extension of the offset attribute file</td>
</tr>
</tbody>
</table>


## <a name="examples"></a>Examples
[Go to Top](#top)  

### Global Header
	{
	Identifier: "NGPF",
	Version: "1.0.0.3fa5735-dirty",
	TimeSteps: 100,
	SimulationTimeUnit: "nanoseconds",
	MaxSimulationBox: [-1.0, -1.0, -1.0, 1.0, 1.0, 1.0],
	TypeHeader: "typeheader.json",
	TimeStepHeader: "timestepheader.json",
	TimeStepDirectoryPrefix: "timestep%.3i",
	TimeStepDirectoryIncrement: 10,
	TimeStepAttributeExtension: "dat",
	TimeStepLayoutColumnCount: 6,
	TimeStepLayoutColumnName: ["x", "y", "z", "r", "g", "b"],
	TimeStepLayoutColumnType: ["float", "float", "float", "byte", "byte", "byte"]
	}

The *parameter* `TimeStepDirectoryPrefix: "timestep%0.3i"` fixes the time step directory affix to a three digit integer.
This results in the directory names `timestep000`, `timestep010`, `timestep020`, ...
### Time Step Header
	{
	TimeStepID: 0,
	SimulationTime: 0.0,
	Particles: 1000,
	SimulationBox: [-1.0, -1.0, -1.0, 1.0, 1.0, 1.0],
	ParameterOffsets: [0, 0, 0, 0, 0, 0]],
	Codecs:	[
		{name: "ZFP",
	 	 epsilon: 0.1},
		{name: "ZFP",
		 epsilon: 0.1},
		{name: "ZFP",
	 	 epsilon: 0.1},
		{name: "RAW",
	 	 encoding: "littleEndian"},
		{name: "RAW",
	 	 encoding: "littleEndian"},
		{name: "RAW",
	 	 encoding: "littleEndian"}
		]
	}{
	TimeStepID: 1,
	SimulationTime: 0.1,
	Particles: 1000,
	SimulationBox: [1.0, 1.0, 1.0],
	ParameterOffset: [32000, 32000, 32000, 8000, 8000, 8000]],
	Codecs:	[
		{name: "ZFP",
	 	 epsilon: 0.1},
		{name: "ZFP",
		 epsilon: 0.1},
		{name: "ZFP",
	 	 epsilon: 0.1},
		{name: "RAW",
	 	 encoding: "littleEndian"},
		{name: "RAW",
	 	 encoding: "littleEndian"},
		{name: "RAW",
	 	 encoding: "littleEndian"}
		]
	}

### Type File (optional)
	{
	TypeID: 0,
	Name: "H",
	NumberSites: 1
	Color: [r, g, b, a],
	Radius: 1.2,
	}{
	TypeID: 1,
	Name: "O",
	NumberSites: 1
	Color: [r, g, b, a],
	Radius: 2.2,
	}{
	TypeID: 2,
	Name: "C",
	NumberSites: 1
	Color: [r, g, b, a],
	Radius: 2.4,
	}{
	TypeID: 3,
	Name: "OH",
	NumberSites: 2,
	Centers: [[x1, y1, z1], [x2, y2, z2]],
	Types: [1, 0]
	}{
	TypeID: 4,
	Name: "Methanol",
	Color: [r, g, b, a],
	NumberSites: 5,
	Types: [0, 0, 0, 2, 3],
	Centers: [[x1, y1, z1], [x2, y2, z2], [x3, y3, z3], [x4, y4, z4], [x5, y5, z5]],
	Quaternions: [[], [], [], [], [qr, qi, qj ,qk]]
	}

### Domain Decomposition Header (optional)

	{
	TimeStepID: 0,
	Domains: 8,
	DDOffsetExtension: "offset",
	DDBoxes: [
		 [-1.0, -1.0, -1.0, 0.0, 0.0, 0.0],
		 [-1.0, -1.0,  0.0, 0.0, 0.0, 1.0],
		 [-1.0,  0.0, -1.0, 0.0, 1.0, 0.0],
		 [-1.0,  0.0,  0.0, 0.0, 1.0, 1.0],
		 [ 0.0, -1.0, -1.0, 1.0, 0.0, 0.0],
		 [ 0.0, -1.0,  0.0, 1.0, 0.0, 1.0],
		 [ 0.0,  0.0, -1.0, 1.0, 1.0, 0.0],
		 [ 0.0,  0.0,  0.0, 1.0, 1.0, 1.0]]
	}

[ZFP]: (https://github.com/Unidata/compression/tree/master/zfp)
[JSON]: (http://www.json.org)
