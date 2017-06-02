# File Format Specification NGPF
Version: 1.0  
VISUS University of Stuttgart  
Oliver Fernandes, Patrick Gralka, Tobias Rau

## Preface
[File format](#file-format)  
[Global header](#global-header)  
[Frame header](#frame-header)  
[Frame data](#frame-data)  
[Type file](#type-file)  
[Example](#example)

## File format 
[comment]: # (General description of the file format + overview picture)
[Go to Top](#file-format-specification-ngpf)  
The global header contains information that is relevant for all stored frames.


## Global header
[Go to Top](#file-format-specification-ngpf)  
The global header format is ASCII and it contains a number of properties:

<table style="width:97%;">
<caption> Parameters accepted by the NGPF global header.</caption>
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
<tr class="odd">
<td align="left"><code>Identifier</code></td>
<td align="left"><code>string</code></td>
<td align="left">identifies the file as NGPF global header</td>
</tr>
<tr class="even">
<td align="left"><code>Version</code></td>
<td align="left"><code>int.int.int</code></td>
<td align="left">NGPF version number</td>
</tr>
<tr class="odd">
<td align="left"><code>Frames</code></td>
<td align="left"><code>int</code></td>
<td align="left">number of frames</td>
</tr>
<tr class="even">
<td align="left"><code>BoundingBox</code></td>
<td align="left"><code>float float float</code></td>
<td align="left">maximum bounding box of the data set</td>
</tr>
<tr class="odd">
<td align="left"><code>ClippingBox</code></td>
<td align="left"><code>float float float</code></td>
<td align="left">maximum clipping box of the data set</td>
</tr>
<tr class="even">
<td align="left"><code>FrameHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the frame header (see <a href="#frame-header">Frame header</a>) </td>
</tr>
<tr class="odd">
<td align="left"><code>TypeHeader</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the frame header (see <a href="#type-header">Type header</a>)</td>
</tr>
<tr class="even">
<td align="left"><code>FrameLayoutColumnCount</code></td>
<td align="left"><code>int</code></td>
<td align="left">number of columns per frame</td>
</tr>
<tr class="odd">
<td align="left"><code>FrameLayoutColumnName</code></td>
<td align="left"><code>string string string ...</code></td>
<td align="left">name of each column</td>
</tr>
<tr class="even">
<td align="left"><code>FrameLayoutColumnCodec</code></td>
<td align="left"><code>string string string ...</code></td>
<td align="left">name of the codec used to compress each column</td>
</tr>
</tbody>
</table>

The values are space separated from the corresponding keyword e.g. 
    
	Idendifier hurtZ_Crowbar
	Version 1.0.0
    Frames 10
	...

## Frame header
[Go to Top](#file-format-specification-ngpf)  
The frame header format is ASCII and it contains a number of properties:

<table style="width:97%;">
<caption> Parameters accepted by the NGPF frame header.</caption>
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
<tr class="odd">
<td align="left"><code>Particles</code></td>
<td align="left"><code>int</code></td>
<td align="left">number of particles</td>
</tr>
<tr class="even">
<td align="left"><code>FrameID</code></td>
<td align="left"><code>int</code></td>
<td align="left">ID of the frame, usually the first frame has id 0</td>
</tr>
<tr class="odd">
<td align="left"><code>Timestamp</code></td>
<td align="left"><code>float</code></td>
<td align="left">corresponding time to the frame</td>
</tr>
<tr class="even">
<td align="left"><code>BoundingBox</code></td>
<td align="left"><code>float float float</code></td>
<td align="left">bounding box of frame</td>
</tr>
<tr class="odd">
<td align="left"><code>ClippingBox</code></td>
<td align="left"><code>float float float</code></td>
<td align="left">clipping box of the frame</td>
</tr>
<tr class="even">
<td align="left"><code>SimulationBox</code></td>
<td align="left"><code>float float float</code></td>
<td align="left">simulation box of the frame</td>
</tr>
<tr class="odd">
<td align="left"><code>FrameFile</code></td>
<td align="left"><code>string</code></td>
<td align="left">Relative path to the frame data file (see <a href="#frame-data">Frame data</a>)</td>
</tr>
<tr class="even">
<td align="left"><code>FrameOffset</code></td>
<td align="left"><code>int</code></td>
<td align="left">offset inside the data file to the corresponding frame</td>
</tr>
</tbody>
</table>


## Frame data
[Go to Top](#file-format-specification-ngpf)  
[comment]: # (Description of the frame data structure)



## Type file
[Go to Top](#file-format-specification-ngpf)  
[comment]: # (Identification of type ids with atoms or molecules)


## Example
[Go to Top](#file-format-specification-ngpf)  
[comment]: # (Minimal example for each file)

[ZFP]: (https://github.com/Unidata/compression/tree/master/zfp)