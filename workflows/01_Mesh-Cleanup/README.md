# Mesh Cleanup

## Description

This section is about cleaning up and repairing the tessellated 3D model to get a solid basis for further processing and workflows.  

Within the RapidPipeline 3D Processor CLI the Mesh Cleanup features as described in this repository can be found under the [3D Edit](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings) section.  

### Example Files

[28L Storage Box - Assembly.x_t](<../../sample-assets/28L Storage Box - Assembly.x_t/README.md>)  
[Download Link](https://grabcad.com/library/28l-30-qt-storage-box-sterilite-ultra-latch-1985lab86-1)  
[<img src="../../sample-assets/28L Storage Box - Assembly.x_t/screenshot/28L Storage Box - Assembly.jpg" width="400">](<../../sample-assets/28L Storage Box - Assembly.x_t/README.md>)  

[Robot rv.IGS](<../../sample-assets/Robot rv.IGS/README.md>)  
[Download Link](https://grabcad.com/library/mitsubishi-rv-2f-d1-s16-6-axis-robot-arm-1)  
[<img src="../../sample-assets/Robot rv.IGS/screenshot/Robotrv.jpg" width="400">](<../../sample-assets/Robot rv.IGS/README.md>)  

[no.468 gt4rs.stp](<../../sample-assets/no.468 gt4rs.stp/README.md>)  
[Download Link](https://grabcad.com/library/porsche-718-cayman-gt4-rs-1)  
[<img src="../../sample-assets/no.468 gt4rs.stp/screenshot/no.468 gt4rs.jpg" width="400">](<../../sample-assets/no.468 gt4rs.stp/README.md>)  

[WRE 45 ASS TOTAL.x_t](<../../sample-assets/WRE 45 ASS TOTAL.x_t/README.md>)  
[Download Link](https://grabcad.com/library/refrigerator-wre45-1)  
[<img src="../../sample-assets/WRE 45 ASS TOTAL.x_t/screenshot/WRE 45 ASS TOTAL.x_t.jpg" width="400">](<../../sample-assets/WRE 45 ASS TOTAL.x_t/README.md>)  

## Sample Results

The sample results can be found within the [sub-directory here](./sample-results)  

28L Storage Box - Assembly.x_t recalculated mesh normals:  
<img src="sample-results/screenshot/28L Storage Box - Assembly_normal-recalculation.jpg" width="400">  
<br>
Robot rv.IGS corrected winding order (wrong winding order indicated in red):  
<img src="sample-results/screenshot/Robot rv.IGS_windingOrderCorrection.jpg" width="400">  
<br>
no.468 gt4rs.stp Merged vertices on rear window (unconnected parts are illustrated in random wireframe color values):  
<img src="sample-results/screenshot/no.468 gt4rs.stp_merged-verts_wire.jpg" width="400">  
no.468 gt4rs.stp corrected winding order:  
<img src="sample-results/screenshot/no.468 gt4rs.stp_windingOrderCorrection.jpg" width="400">  
Note: Sewing was performed before tessellation (see [previous workflow section](../00_CAD-Data-Ingestion/README.md#sewing-tolerance) for details regarding CAD import).  
<br>
WRE 45 ASS TOTAL.x_t Recalculated mesh normals & corrected winding order:  
<img src="sample-results/screenshot/WRE 45 ASS TOTAL_normal-recalculation.jpg" width="400">  
WRE 45 ASS TOTAL.x_t Winding Order Correction Challenges (more regarding the particular [challenges for this asset here](<../../sample-assets/WRE%2045%20ASS%20TOTAL.x_t/README.md#purpose>)):  
<img src="sample-results/screenshot/WRE 45 ASS TOTAL_winding-order-correction.jpg" width="400">  

## Steps to Reproduce

In order to reproduce the given results please follow the steps below:

### 3D Processor CLI

1. [Install and set-up the RapidPipeline 3D Processor CLI](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide)  
	- Requires RapidPipeline enterprise plan or free enterprise trial to access the CLI ([Contact here](https://rapidpipeline.com/en/contact/))  
	- Information regarding [latest version and changelog can be found here](https://docs.rapidpipeline.com/3d-processor-updates)  
2. Download the respective example file for this tutorial. You can find the files in the [overview here](../README.md).  
3. Get the respective .json settings configuration file further below and make sure input file as well as .json file are present  
4. Run the command listed below in your favorite commandline (e.g. windows powershell), more about [3D Processor CLI commands here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#commands-guide)  

Further information regarding [Mesh Cleanup (3D Edit) Settings](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings) in the [RapidPipeline Documentation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/3d-processor-overview).  

## Commands

### Normal Recalculation

```
rpdx --read_config cleanup-normals.json -i '28L Storage Box - Assembly (Sterilite UltraLatch 1985LAB86).x_t' -r
```

### Vertex Merging

```
rpdx --read_config vertex-merging.json --read_config rotateZUp.json -i 'no.468 gt4rs.stp' -r
```

### Winding Order Correction

```
rpdx --read_config cleanup-windingOrder.json -i 'Robot rv.IGS' -r
```

#### Utilizing Vertex Merging and Winding Order Correction

```
rpdx --read_config vertex-merging.json --read_config cleanup-windingOrder.json --read_config rotateZUp.json -i 'no.468 gt4rs.stp' -r -e 'output_VertMergWindOrder/no.468 gt4rs.usd'
```

#### Utilizing Normal Recalculation, Vertex Merging and Winding Order Correction

```
rpdx --read_config combined-workflow.json -i 'WRE 45 ASS TOTAL.x_t' -r -e 'output_normalRecVertMergWindOrder/WRE 45 ASS TOTAL.usd'
```


Note: Within the configuration .json settings files in this repository only `usd` output formats are specified. RapidPipeline [supports a lot more file formats](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/format-support) which can be [configured within the settings file](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7#export-slot).  

The 3D Processor CLI will automatically create a default output folder if the run command `-r` is used. In order to define a specific output path the command `-o` can be utilized instead. Read more about [file exports with the CLI here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-a-3d-file).  

Alternatively the export command `-e` can be used to generate any output path or file format.  Read more about the [export command here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-via-command).  

Multiple settings files can be combined as seen in the second and fourth command line examples above. At least one of the configuration files needs to declare an output format within the `export` section (unless the `-e` command is utilized).  

## Settings File

The settings file is the basis for all processing with the RapidPipeline 3D Processor CLI.  
It is following `.json` syntax and is validated against the [RapidPipeline 3D Processing Schema](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7).  

In this example we are only making use of the `Import`, `3D Edit` and `Export` sections (`objects`).  
However, there are a lot more options for 3D processing. More about combining more sections within a single settings .json file in the [Batch Processing workflow](../06_Batch-Processing/README.md).  


## Features & Settings - Main Concepts

Please see below some basic explanation for each feature and setting. Each header also contains a link to the respective sections of the RapidPipeline Documentation:  

### [Normal (Re)computation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#mesh-normals)

Mesh normals control the shading of meshes. There are two controls: the Normal Hard Angle Threshold, and the Normal Computation Method. These settings are applied just after Import.  

#### [Recompute Input Normals](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#mesh-normals)

Discards the original normals and recomputes them.  

#### [Normal Hard Angle Threshold (Degrees)](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#normal-hard-angle-threshold)

Hard threshold (degrees) used for normal generation (0 = everything flat, 180 = everything smooth)  

#### [Normal Computation Method](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#normal-computation-method)

The method affects how the angles of the normals for soft edges are handled.  


### [Vertex Merging](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#vertex-merging)

Note: If the vertex merging distance is `0`, only duplicated vertices removal will be performed.  

#### [Vertex Merging Per Mesh](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#vertex-merging)

Vertex Merging Per Mesh decides if vertex merging should only be performed within each mesh node (on) or across all nodes (off). 

#### [Vertex Merging Distance Percentage](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#vertex-merging)

Merges close vertices with a given threshold in percentage.  

#### [Vertex Merging Distance Value](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#vertex-merging)

Merges close vertices with a given threshold as absolute value (meters).  

#### [Merge Meshes](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#vertex-merging)

The merge meshes option allows for the different meshes of merged vertices to be actually merged (vertices are welded) together.  



### [Fix Winding Order](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#fix-winding-order)

Automatically fix the winding order of parts on import.  

#### [Visibility Mode](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#fix-winding-order)

Set how visibility is computed  
- default: Triangle facing is checked on a global basis. This is the default value  
- mesh: Triangle facing is checked individually per mesh node  

#### [Winding Order Ignore Transparency](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#fix-winding-order)

Decides whether visibility includes non-opaque meshes.  

#### [Winding Order Per Lump](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-3dedit-settings#fix-winding-order)

Decides whether winding order of whole (mesh) lumps of geometry are flipped as one or per triangle.  


### Download or copy the settings file

#### Normal Recalculation

[cleanup-normals.json](cleanup-normals.json)

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
  "3dEdit": {
    "meshNormals": {
      "recomputeInputNormals": true,
      "hardAngleThreshold": 60.0,
      "computationMethod": "area"
    },
    "modelEdit": {
        "splitMultiMaterialMeshes": true
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
                "enable" : false
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

#### Vertex Merging

[vertex-merging.json](vertex-merging.json)

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
  "3dEdit": {
    "modelEdit": {
        "splitMultiMaterialMeshes": true
    },
    "repair" : {
      "vertexMerging":{
        "mergeDistance": {
          "percentage": 0.005
        },
        "perMesh": false 
      }
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
                "enable" : false
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

#### Winding Order Correction

[cleanup-windingOrder.json](cleanup-windingOrder.json)

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
  "3dEdit": {
    "meshNormals": {
      "recomputeInputNormals": false
    },
    "modelEdit": {
        "splitMultiMaterialMeshes": true
    },
    "repair": {
      "windingOrder": {
        "visibilityMode": "default",
        "ignoreTransparency" : false,
        "perLump": true
      }
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
                "enable" : false
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

#### Combined Workflow

[combined-workflow.json](combined-workflow.json)

```
{
    "import": {
      "CAD": {
        "tessellationResolution":"extraFine", 
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
  "3dEdit": {
    "meshNormals": {
      "recomputeInputNormals": true,
      "hardAngleThreshold": 60.0,
      "computationMethod": "area"
    },
    "modelEdit": {
        "splitMultiMaterialMeshes": false
    },
    "repair" : {
      "vertexMerging":{
        "mergeDistance": {
          "percentage": 0.0
        },
        "perMesh": false 
      },
      "windingOrder": {
        "visibilityMode": "default",
        "ignoreTransparency" : false,
        "perLump": true
      }
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
                "enable" : false
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