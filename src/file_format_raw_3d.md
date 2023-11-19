# File Format: RAW 3D

RAW 3D files are just plain 3D arrays of voxel value data. These files do not contain a header with metadata on how to read or interpret the data. While VDS has a well defined [Volume Data Representation](/about_volume_data.html#volume-data-representation-in-vds), there is no universal applicable standard for memory layout, number type, voxel order, endianess and so on when storing volume data. Therefore, you need to provide the nessesary metadata on how to read and interpret the data when importing a RAW 3D file.

## Typical file extensions:

- *.raw
- *.RAW

## Supported Number Types for Voxels

Each voxel value can be stored as a signed integer, an unsigned integer or as a floating point number. Each of these number types can be stored with various amounts of bits per number, which is known as _"precision"_. A higher precision usually means more details in the image data at the cost of larger file sizes. The following number types are supported by VDS when importing a RAW 3D file:

ID  | Number Type           | Precision
--- | --------------------- | ---------
u8  | Unsigned Integer      | 8 Bit
u16 | Unsigned Integer      | 16 Bit
i8  | Signed Integer        | 8 Bit
i16 | Signed Integer        | 16 Bit
f16 | Floating Point Number | 16 Bit

> Internally, VDS always converts voxel values to f16 before beeing operated on.

## Scaling

VDS supports uniform and non-uniform spacing with different scaling factors for each axis. Non-linear scaling is not supported.

## Voxel Ordering

Some file formats like RAW 3D do not store metadata about the coordinate system. It could be possible, that for example the RAW 3D files content was stored in RAS but the VDS viewer is configured to use LAS.

**Storage Orders** desribe the voxel ordering. 

Here is an example for the Storage Order for **LAS** which would be _"R->L within P->A within I->S"_ and means:

1. Voxels ordered from right to left to store a row
2. Rows ordered from posterior to anterior to store a slice
3. Slices stored from inferior to superior to store a volume

By selecting the correct Storage Order, the axis of the file data coordinate system will be mapped correctly to configured VDS viewing coordinate system.

VDS supportes the following voxel orderings for RAW 3D files:

[Axes for Spatial Coordinates](/about_volume_data.html#axes-for-spatial-coordinates) | Storage order in file        | [Slice orientation](/about_volume_data.html#planes-for-volume-slice-orientation) (ambiguous) | Known as
------------------------------------------------------------------------------------ | ---------------------------- | -------------------------------------------------------------------------------------------- | ------------
LAS                                                                                  | R->L within P->A within I->S | "Axial"                                                                                      | Radiological
RAS                                                                                  | L->R within P->A within I->S | "Axial"                                                                                      | Neurological
LSA                                                                                  | R->L within I->S within P->A | "Saggital"                                                                                   |
LIA                                                                                  | R->L within S->I within P->A | "Coronal"

> If you select the wrong voxel order while importing a RAW 3D file, then you might see the volume rotated or mirrored along any of the spatial axes of the volume and not mapped correctly to the [Axes for Spatial Coordinates](/about_volume_data.html#axes-for-spatial-coordinates).

### Converting RAW 3D File Voxel Ordering to an In-Memory Array

Most RAW 3D files are stored as [RAS](/about_volume_data.html#axes-for-spatial-coordinates):

1. _X increases from Left to Right_
2. _Y increases from Posterior to Anterior_
3. _Z increases from Inferior to Superior_

A naive approach to actually read an RAW 3D file into memory in one big blob and treat it as an array would probably result in a wrong access pattern like: Voxels[ X ][ Y ][ Z ].

Popular programming languages like C/C++/Rust use the first index as the slowest-incrementing index into memory. The correct access pattern for this example would be Voxels[ Z ][ Y ][ X ].

## Alignment to Axes for Spatial Coordinates

Sometimes the voxel data in RAW 3D files is not aligned to some exact orthogonal directions. Nonetheless, it's useful to know which set of axes the voxel indices correspond to most closely, as this helps when applying alignment or rotation steps.

Furthermore, the location of the origin is unkown. Even if we know the origin voxel, the location of the origin can be centered in the middle of this central voxel, or on the one of its eight corners.

VDS defaults the origin for RAW 3D files to the center of the volume 3D dimensions after the [Scaling](#scaling) is applied.

## Multiple volumes in a single file

Multiple volumes in a single file could be used for multiple time points. VDS does not support multiple volumes in a single file when importing a RAW 3D file.
