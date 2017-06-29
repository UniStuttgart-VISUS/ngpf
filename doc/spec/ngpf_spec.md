# <a name="top"></a>File Format Specification NGPF
Version:  
VISUS University of Stuttgart  
Oliver Fernandes, Patrick Gralka, Tobias Rau

## Sections
[Preface](#preface)  
[General Conventions](#general-conventions)  
[Global Header](#global-header)  
[Frame Header](#frame-header)  
[Data](#data)  
[Type File](#type-file)  
[Example](#example)

## <a name="preface"></a>Preface 
This is a particle format that focuses on easy *readability for meta data* and *high compression* for everything else.
The NGPF file format offers human-readable [JSON] headers and a separation of global and per-frame parameters.
The user can use the header files for quick reference of the relevant parameters.
The actual data is stored in a *binary format*.
The user can choose from a variety of binary codecs to encode the data (including RAW, i.e. unencoded).
The NGPF API offers a variety of *binary codecs* (link to the NGPF API).
The components of the NGPF file format and their interactions are shown in the schematic below.
A *Global Header* stores the information that applies for all frames, such as *Version number* or the *number of frames*.
The *Frame Header* contains the meta data of each individual frame (e.g. *number of particles*, *time stamp*).
An optional *Type File* can be used to assign chemical elements to particles or describe rigid molecules (no inner DOF) via multiple sites/centers per particle (which should then be interpreted as instances of the rigid molecules).
Additionally, the Type File can contain meta information about the element/molecule.

<center>
![](schematic.png "")
Figure: Schematic of the file format header management.
</center>  

## <a name="general-conventions"></a>General Conventions
[Go to Top](#top)  
In the following sections we use some terms that need a clear definition.
The term `float` corresponds to a IEEE-754 standard floating point value with a size of 32 bit.
A `byte` is a unsigned integer with the size of 8 bit.
The NGPF headers will handle a `string` as a unicode string.
A string that represents a relative file path always uses forward slashes `/` to separate directories.
The `Version` of the NGPF file format consists of three integer values separated with a dot (`int.int.int`).
The first integer corresponds to the major format version and is not necessarily compatible with the previous version, the second integer is incremented at feature releases, and the last integer is incremented at smaller changes and bug fixes. Integer `int` and float `float` values also support the scientific number format (e.g. `1e2` for `int` and `float` or `1.0e2(.0)` only for `float`). A parameter can be something obvious as `x` or `z`, but also an interleaved combination of all `xyz`.
  
  

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
<td align="left"><code>Frames</code></td>
<td align="left"><code>int</code></td>
<td align="left">Number of frames</td>
</tr>
<tr>
<td align="left"><code>TimeStampUnit</code></td>
<td align="left"><code>string</code></td>
<td align="left">Unit of the <code>TimeStamp</code> parameter of the frame headers, e.g. <code>seconds</code>, <code>custom</code></td>
</tr>
<tr>
<td align="left"><code>MaxSimulationBox</code></td>
<td align="left"><code>[float, float, float]</code></td>
<td align="left">Maximum simulation box of the data set</td>
</tr>
<tr>
<td align="left"><code>TypeHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the frame header (see <a href="#type-header">Type File</a>)</td>
</tr>
<tr>
<td align="left"><code>FrameHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the frame header (see <a href="#frame-header">Frame Header</a>) </td>
</tr>
<tr>
<td align="left"><code>FrameDirectoryPrefix</code></td>
<td align="left"><code>string</code></td>
<td align="left">Sets the naming scheme for the frame directories</td>
</tr>
<tr>
<td align="left"><code>FrameDirectoryIncrement</code></td>
<td align="left"><code>int</code></td>
<td align="left">Defines how many frames are stored in one directory</td>
</tr>
<tr>
<td align="left"><code>FrameParameterSuffix</code></td>
<td align="left"><code>string</code></td>
<td align="left">Defines the suffix that is appended on each parameter file inside the frame directories</td>
</tr>
<tr>
<td align="left"><code>FrameLayoutColumnName</code></td>
<td align="left"><code>[string, string, ...]</code></td>
<td align="left">Name of each column</td>
</tr>
<tr>
<td align="left"><code>FrameLayoutColumnType</code></td>
<td align="left"><code>[string, string, ...]</code></td>
<td align="left">Name of the column type (e.g. <code>float, int</code>)</td>
</tr>
</tbody>
</table>


## <a name="frame-header"></a>Frame Header
[Go to Top](#top)  


<table style="width:97%;">
<caption> Parameters accepted by the NGPF Frame Header.</caption>
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
<td align="left"><code>FrameID</code></td>
<td align="left"><code>int</code></td>
<td align="left">ID of the frame, usually the first frame has id 0</td>
</tr>
<tr>
<td><code>Particles</code></td>
<td align="left"><code>int</code></td>
<td align="left">Number of particles</td>
</tr>
<tr>
<td align="left"><code>TimeStamp</code></td>
<td align="left"><code>float</code></td>
<td align="left">Corresponding time to the frame</td>
</tr>
<tr>
<td align="left"><code>SimulationBox</code></td>
<td align="left"><code>[float, float, float]</code></td>
<td align="left">Simulation box of the frame</td>
</tr>
</tr>
<tr>
<td align="left"><code>ParameterOffsets</code></td>
<td align="left"><code>[int, int, ...]</code></td>
<td align="left">Offset inside the data file to the corresponding frame for each parameter.</td>
</tr>
<tr>
<td align="left"><code>Codecs</code></td>
<td align="left"><code>[struct, struct, ...]</code></td>
<td align="left">Defines a struct for each parameter. A struct contains the codec name and  </td>
</tr>
</tbody>
</table>


## <a name="data"></a>Data
[Go to Top](#top)  

Each frame range is in a separate directory.
Inside these directories, the data is split into a individual file for each parameter.
For example in the `x.dat` file, which is located in the `frame0` directory, the data of parameter `x` for frames `0-9` is stored.
The stored data is compressed by an algorithm is set by the user.
Additionally, this compression algorithm can vary for each frame and each parameter.

<table style="width:96%;">
<caption><b>directory for fame 0 to frame 9</b><caption>
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
<td align="center">ZFP0(x_frame0)</td>
<td align="center">ZFP0(y_frame0)</td>
<td align="center">ZFP0(z_frame0)</td>
<td align="center">RAW0(r_frame0)</td>
<td align="center">RAW0(g_frame0)</td>
<td align="center">RAW0(b_frame0)</td>
</tr>
<tr>
<td align="center">ZFP1(x_frame1)</td>
<td align="center">ZFP1(y_frame1)</td>
<td align="center">ZFP1(z_frame1)</td>
<td align="center">RAW1(r_frame1)</td>
<td align="center">RAW1(g_frame1)</td>
<td align="center">RAW1(b_frame1)</td>
</tr>
<tr>
<td align="center">ZFP2(x_frame2)</td>
<td align="center">ZFP2(y_frame2)</td>
<td align="center">ZFP2(z_frame2)</td>
<td align="center">RAW2(r_frame2)</td>
<td align="center">RAW2(g_frame2)</td>
<td align="center">RAW2(b_frame2)</td>
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
<caption><b>directory for fame 10 to frame 19</b><caption>
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
<td align="center">ZFP10(x_frame10)</td>
<td align="center">ZFP10(y_frame10)</td>
<td align="center">ZFP10(z_frame10)</td>
<td align="center">RAW10(r_frame10)</td>
<td align="center">RAW10(g_frame10)</td>
<td align="center">RAW10(b_frame10)</td>
</tr>
<tr>
<td align="center">ZFP11(x_frame11)</td>
<td align="center">ZFP11(y_frame11)</td>
<td align="center">ZFP11(z_frame11)</td>
<td align="center">RAW11(r_frame11)</td>
<td align="center">RAW11(g_frame11)</td>
<td align="center">RAW11(b_frame11)</td>
</tr>
<tr>
<td align="center">ZFP12(x_frame12)</td>
<td align="center">ZFP12(y_frame12)</td>
<td align="center">ZFP12(z_frame12)</td>
<td align="center">RAW12(r_frame12)</td>
<td align="center">RAW12(g_frame12)</td>
<td align="center">RAW12(b_frame12)</td>
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
If particles are identified by a type ID, the type ID to element or molecule conversion is stored in the type file. Additionally, the file can contain meta information about the molecule, e.g. charge.

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
<td align="left"><code>[[x0, y0, z0],[...],...]</code></td>
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
<td align="left"><code>[[qr,qi,qj,qk],[...],...]</code></td>
<td align="left">Supported, Multisite</td>
<td align="left">Quaternions of connected sites. Can also be empty if a site is e.g. a single atom.</td>
</tr>
</tbody>
</table>


## <a name="example"></a>Example
[Go to Top](#top)  

### Global Header
	{
	Identifier:	"NGPF",
	Version: "1.0.0.3fa5735-dirty",
	Frames : 100,
	TimeStampUnit: "seconds",
	MaxSimulationBox: [1.0, 1.0, 1.0],
	TypeHeader: "typeheader.json",
	FrameHeader: "frameheader.json",
	FrameDirectoryPrefix: "frame%0.3i",  |--> fixes the frame directory affix to a three digit integer
	FrameDirectoryIncrement: 10,         |--> results in frame000, frame010, frame020, ...
	FrameParameterSuffix: "dat",
	FrameLayoutColumnCount: 6,
	FrameLayoutColumnName: ["x", "y", "z", "r", "g", "b"],
	FrameLayoutColumnType: ["float", "float", "float", "byte", "byte", "byte"]
	}

### Frame Header
	{
	FrameID: 0,
	TimeStamp: 0.0,
	Particles: 1000,
	SimulationBox: [1.0, 1.0, 1.0],
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
	FrameID: 1,
	TimeStamp: 0.1,
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

### Type File
	{
	TypeID: 0, # Required
	Name: "H", # Required
	NumberSites: 1
	Color: [r,g,b,a],
	Radius: 1.2,
	}{
	TypeID: 1,
	Name: "O",
	NumberSites: 1
	Color: [r,g,b,a],
	Radius: 2.2,
	}{
	TypeID: 2,
	Name: "C",
	NumberSites: 1
	Color: [r,g,b,a],
	Radius: 2.4,
	}{
	TypeID: 3,
	Name: "OH",
	NumberSites: 2,
	Centers: [[x1,y1,z1],[x2,y2,z2]], #Relative to the center of mass (actual particle 
	position)
	Types: [1,0]
	}{
	TypeID: 4,
	Name: "Methanol",
	Color: [r,g,b,a],
	NumberSites: 5,
	Types: [0,0,0,2,3],
	Centers: [[x1,y1,z1],[x2,y2,z2],[x3,y3,z3],[x4,y4,z4],[x5,y5,z5]],
	Quaternions: [[], [], [], [], [qr, qi, qj ,qk]]
	}

[ZFP]: (https://github.com/Unidata/compression/tree/master/zfp)
[JSON]: (http://www.json.org)
