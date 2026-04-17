# Mesh Simplification & Remeshing

## Description

This section is about Mesh Simplification automation such as Decimation or Remeshing operations.  
Simplifying geometry to improve performance or recreating surfaces for better editing.  

### Example Files

[Cooper CAD refined.step](<../../sample-assets/Cooper CAD refined.step/README.md>)  
[Download Link](https://grabcad.com/library/cooper-quadruped-robot-robot-dog-1)  
[<img src="../../sample-assets/Cooper CAD refined.step/screenshot/cooper-quadruped-robot-robot-dog-1.jpg" width="400">](<../../sample-assets/Cooper CAD refined.step/README.md>)  

[no.468 gt4rs.stp](<../../sample-assets/no.468 gt4rs.stp/README.md>)  
[Download Link](https://grabcad.com/library/porsche-718-cayman-gt4-rs-1)  
[<img src="../../sample-assets/no.468 gt4rs.stp/screenshot/no.468 gt4rs.jpg" width="400">](<../../sample-assets/no.468 gt4rs.stp/README.md>)  

## Sample Results

The sample results can be found within the [sub-directory here](./sample-results)  

Cooper CAD refined.step decimated to 500,000 faces:  
<img src="sample-results/screenshot/Cooper CAD refined_decimated-500k-shaded.jpg" width="400">  
<img src="sample-results/screenshot/Cooper CAD refined_decimated-500k-wire.jpg" width="400">  
<br>
no.468 gt4rs.stp decimated to 500,000 faces:  
<img src="sample-results/screenshot/no.468 gt4rs.stp_decimated-500k-shaded.jpg" width="400">  
<img src="sample-results/screenshot/no.468 gt4rs.stp_decimated-500k-wire.jpg" width="400">  


## Steps to Reproduce

In order to reproduce the given results please follow the steps below:

### 3D Processor CLI

1. [Install and set-up the RapidPipeline 3D Processor CLI](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide)  
	- Requires RapidPipeline enterprise plan or free enterprise trial to access the CLI ([Contact here](https://rapidpipeline.com/en/contact/))  
	- Information regarding [latest version and changelog can be found here](https://docs.rapidpipeline.com/3d-processor-updates)  
2. Download the respective example file for this tutorial. You can find the files in the [overview here](../README.md).  
3. Get the respective .json settings configuration file further below and make sure input file as well as .json file are present  
4. Run the command listed below in your favorite commandline (e.g. windows powershell), more about [3D Processor CLI commands here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#commands-guide)  

Further information regarding Mesh Simplification ([Decimation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-optimize-settings#decimator), [Remeshing](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-optimize-settings#remesher) in the [RapidPipeline Documentation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/3d-processor-overview).  

## Commands

### Decimation

```
rpdx --read_config decimation.json -i 'Cooper CAD refined.step' -r  
```

#### Decimation - Combined Workflow

Decimation combined with vertex merging and winding order correction:

```
rpdx --read_config decimation_combined-workflow.json -i 'no.468 gt4rs.stp' -r  
```

### Remeshing

```
rpdx --read_config remeshing.json -i '' -r
```

Note: Within the configuration .json settings files in this repository only `usd` output formats are specified. RapidPipeline [supports a lot more file formats](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/format-support) which can be [configured within the settings file](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7#export-slot).  

The 3D Processor CLI will automatically create a default output folder if the run command `-r` is used. In order to define a specific output path the command `-o` can be utilized instead. Read more about [file exports with the CLI here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-a-3d-file).  

Alternatively the export command `-e` can be used to generate any output path or file format.  Read more about the [export command here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-via-command).  

## Settings File

The settings file is the basis for all processing with the RapidPipeline 3D Processor CLI.  
It is following `.json` syntax and is validated against the [RapidPipeline 3D Processing Schema](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7).  

In this example we are only making use of the `Import`, `Optimize`, `Scene Graph Flattening` and `Export` sections (`objects`).  
However, there are a lot more options for 3D processing. More about combining more sections within a single settings .json file in the [Batch Processing workflow](../06_Batch-Processing/README.md).  

<!-- 
## Features & Settings - Main Concepts

Please see below some basic explanation for each feature and setting. Each header also contains a link to the respective sections of the RapidPipeline Documentation:  



### [Mesh Culling](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings)

Various methods of culling geometry from an input model.  

### [Occlusion Culling](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#occlusion-culling)

Removes occluded (invisible) parts from the 3D model, for the whole 3D model or individually for each mesh.  

#### [Culling Per Mesh](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#per-mesh)

This causes visibility to only be checked within the same mesh node, and not across the whole (selected) asset.  

#### [Culling Quality](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#culling-quality)

Specify how thorough triangle visibility should be determined.  

#### [Ignore Transparency](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#ignore-transparency)

This causes culling to treat transparent geometry as opaque.  

#### [Run after Decimation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#run-after-decimator)

When enabled, occluded triangles will be removed after decimation. Requires an Optimization `object` with `Decimator` section within the same configuration or commandline execution.  

#### [Visibility Diffusion](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#visibility-diffusion)

Control if neighbors of visible triangles are also flagged as visible.  

#### [Sample Edges](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#sample-edges)

Defines if edges should be sampled during visibility computation.  

### [Per Lump Decision (Visibility Per Part)](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#visibility-per-part)

This causes culling to be performed for whole mesh chunks, rather than per triangle.  

### [Lump Threshold](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#visibility-per-part)

Threshold in percent based on number of visible triangles. The default of 0.1 means that if 90% or more triangles per mesh lump are invisible, the whole lump is declared as invisible.  


### [Small Feature Culling](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#small-feature-culling)

Removes small parts from the 3D model. You can select the size threshold in scene units (usually meters) under which small parts should be removed. The algorithm works on a per part (mesh lump) basis.  

#### [Size Threshold](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#size-threshold)

Choice between Relative Percentage (bounding box) or Value (scene units).  


### [Scene Graph Flattening](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-flattening-settings)

Scene Graph Flattening settings allow nodes to be combined according to various properties.  

#### [Method](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-flattening-settings#flattening-methods)

Several methods are provided for controlling how nodes are combined. Material merging is automatically handled when nodes are flattened, depending on the mode.  

#### [Preserved Scene Depth](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-flattening-settings#preserved-scene-depth-level)

This number acts as an overall limit, preserving a minimum number of scene graph levels, by allowing some nodes to be combined while preserving others based on their level in the hierarchy.  
-->

### Download or copy the settings file

#### Decimation

[decimation.json](decimation.json)

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
    }
  },
    "optimize": {
        "3dModelOptimizationMethod": {
            "meshAndMaterialOptimization": {
                "decimator": {
                    "materialOptimization": {
                     "keepMaterialsUVs": {
                        "forceNormalRebaking": false
                      }
                    },
                    "target": {
                        "faces": {
                            "value": 500000
                        },
                        "deviation": {
                            "percentage": 0.005
                        }
                    },
                    "preserveNormals": true,
                    "preserveTopology": false
                }
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

#### Decimation - Combined Workflow

[decimation_combined-workflow.json](decimation_combined-workflow.json)

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
      "rotateZUp": true
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
      "vertexMerging":{
        "mergeDistance": {
          "percentage": 0.01
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
    "optimize": {
        "3dModelOptimizationMethod": {
            "meshAndMaterialOptimization": {
                "decimator": {
                    "materialOptimization": {
                     "keepMaterialsUVs": {
                        "forceNormalRebaking": false
                      }
                    },
                    "target": {
                        "faces": {
                            "value": 500000
                        },
                        "deviation": {
                            "percentage": 0.005
                        }
                    },
                    "preserveNormals": true,
                    "preserveTopology": false
                }
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

#### Remeshing

[remeshing.json](remeshing.json)

```
{

}
```