# CAD Data Ingestion

## Description

This first workflow section is all about how to ingest (import) CAD data into the RapidPipeline Software.  

That process is not merely a simple file import but instead a complex conversion from parametric surface (CAD) to polygonal mesh data (triangles, quads, etc.).  

RapidPipeline offers lots of settings to fine-tune the CAD Ingestion process, while offering reasonable defaults which will make sure any CAD file can be imported and displayed as a mesh as a baseline.  

Please see more information regarding the individual settings in the respective sections further below.  

### Example Files & Results

<br>

The sample results can be found within the [sub-directory here](./sample-results)  

<br>

Tessellated mesh with quad dominant topology:  

| Input CAD Asset | Processed Output |
|---------|-------------|
| [EG 43-17 HG Pojemnik.STEP](<../../sample-assets/EG 43-17 HG Pojemnik.STEP/README.md>), [Download Link](https://grabcad.com/library/eg43-17-hg-1)[<img src="../../sample-assets/EG 43-17 HG Pojemnik.STEP/screenshot/EG 43-17 HG Pojemnik.jpg" width="400">](<../../sample-assets/EG 43-17 HG Pojemnik.STEP/README.md>) | <img src="sample-results/screenshot/EG 43-17 HG Pojemnik-result.jpg" width="400"> |  
<br>
Tessellated 'Robot rv.IGS' asset "raw" import (left) vs with sewing pre-process on the boundary representations (right); Different parts/patches are illustrated in random color values:  

| Input CAD Asset | Processed Output |
|---------|-------------|
| [Robot rv.IGS](<../../sample-assets/Robot rv.IGS/README.md>), [Download Link](https://grabcad.com/library/mitsubishi-rv-2f-d1-s16-6-axis-robot-arm-1)[<img src="../../sample-assets/Robot rv.IGS/screenshot/Robotrv.jpg" width="400">](<../../sample-assets/Robot rv.IGS/README.md>) | <img src="sample-results/screenshot/Robot rv.IGS_sewing.jpg" width="400"> |  


Note: The Robot rv.IGS asset also has some improper winding order after the tessellation process. Please see the [next workflow](../01_Mesh-Cleanup/README.md#fix-winding-order) to learn more about Winding Order and how to correct it if needed.  

## Steps to Reproduce

In order to reproduce the given results please follow the steps below:

### 3D Processor CLI

1. [Install and set-up the RapidPipeline 3D Processor CLI](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide)  
	- Requires RapidPipeline enterprise plan or free enterprise trial to access the CLI ([Contact here](https://rapidpipeline.com/en/contact/))  
	- Information regarding [latest version and changelog can be found here](https://docs.rapidpipeline.com/3d-processor-updates)  
2. Download the respective example file for this tutorial. You can find the files in the [overview here](../README.md).  
3. Get the respective .json settings configuration file further below and make sure input file as well as .json file are present  
4. Run the command listed below in your favorite commandline (e.g. windows powershell), more about [3D Processor CLI commands here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#commands-guide)  

Further information regarding [CAD import settings](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#cad-import-settings) in the [RapidPipeline Documentation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/3d-processor-overview) and [Tutorials](https://docs.rapidpipeline.com/docs/3dProcessor-Tutorials/tutorial-cli-cad).  

## Commands

```
rpdx --read_config CAD-ingestion.json -i 'EG 43-17 HG Pojemnik.STEP' -r -o output
```

```
rpdx --read_config CAD-ingestion.json -i 'Robot rv.IGS' -r -o output
```


Note: Within the configuration .json settings files in this repository only `usd` output formats are specified. RapidPipeline [supports a lot more file formats](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/format-support) which can be [configured within the settings file](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7#export-slot).  

The 3D Processor CLI will automatically create a default output folder if the run command `-r` is used. In order to define a specific output path the command `-o` can be utilized instead. Read more about [file exports with the CLI here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-a-3d-file).  

Alternatively the export command `-e` can be used to generate any output path or file format.  Read more about the [export command here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-via-command).  

## Settings File

The settings file is the basis for all processing with the RapidPipeline 3D Processor CLI.  
It is following `.json` syntax and is validated against the [RapidPipeline 3D Processing Schema](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7).  

In this example we are only making use of the `Import` and `Export` sections (`objects`).  
However, there are a lot more options for 3D processing. More about combining more sections within a single settings .json file in the [Batch Processing workflow](../06_Batch-Processing/README.md).  


### Settings - Main Concepts

Please see below some basic explanation for each setting. Each header also contains a link to the respective sections of the RapidPipeline Documentation:  

#### [Tessellation Resolution](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#tessellation-resolution)

Tessellation resolution for imported CAD surfaces. By default these are presets such as `coarse` and `fine`. The `custom` options enables further control via the `Maximum Surface Deviation`, `Angle`, `EdgeLength` settings.  

#### [Sewing Tolerance](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#cad-sewing-tolerance)

Tolerance for the sewing operation on the b-reps before tessellation.  

#### [Removal of T-Junctions](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#cad-remove-t-junctions)

Attempts to remove T-Junctions after CAD tessellation.  


#### [Maximum Surface Deviation, Angle, EdgeLength](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#expert-tessellation-settings)

- Surface Deviation = Maximum distance between the CAD surface and the tessellation in mm (sometimes also referred to as "Chord Height")  

- Maximum Angle = Decreasing the max angle generates more faces in high curvature areas, such as fillets for example  

- Maximum Edge Length = Controls the maximum length of edges per face. Caution, as the value is absolute (mm), large parts might become over-tessellated, including flat surfaces  


#### [Rotate Z-Up to Y-Up](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-import-settings#convert-z-up-to-y-up)

Turns rotation to z-axis pointing upwards on/off

#### Export Tris to Quads

For this simple CAD to mesh conversion, we enabled tris to quads conversion for the output data. This is currently only supported for `.usd` formats.

### Download or copy the settings file

[CAD-ingestion.json](CAD-ingestion.json)

```
{
    "import": {
      "CAD": {
        "tessellationResolution":"custom", 
        "sewTolerance": 0.05, 
        "removeTJunctions": true,
        "maxSurfaceDeviation": 0.05,
        "maxAngle": 40,
        "maxEdgeLength": 0
      },
    "general": {
      "rotateZUp": false
      }
    },
    "export": [
        {
            "discard": {
                "emptyNodes": true, 
                "unusedUVs": false
            }, 
            "fileName": "", 
            "format": {
                "usd": {
                }
            }, 
            "trisToQuads" : {
                "enable" : true
            },
            "optimizeFaceOrder": true, 
            "preserveTextureFilenames": false, 
            "reencodeTextures": "auto", 
            "textureMapFilePrefix": "", 
            "textureNamingScript": ""
        }
  ]
}
```
