# Mesh Simplification & Remeshing

## Description

This workflow section is about Mesh Simplification automation such as Decimation or Remeshing operations.  
Simplifying geometry to improve performance or re-creating surfaces for better editing.  

## Example Files & Results  

<br>

The sample results can be found within the [sub-directory here](./sample-results)  

<br>

Cooper CAD refined.step decimated to 500,000 faces:  

| Input CAD Asset | Processed Output |
|---------|-------------|
| [Cooper CAD refined.step](<../../sample-assets/Cooper CAD refined.step/README.md>), [Download Link](https://grabcad.com/library/cooper-quadruped-robot-robot-dog-1)[<img src="../../sample-assets/Cooper CAD refined.step/screenshot/cooper-quadruped-robot-robot-dog-1.jpg" width="400">](<../../sample-assets/Cooper CAD refined.step/README.md>) | <img src="sample-results/screenshot/Cooper CAD refined_decimated-500k-shaded.jpg" width="400"><img src="sample-results/screenshot/Cooper CAD refined_decimated-500k-wire.jpg" width="400"> |  
<br>

no.468 gt4rs.stp decimated to 500,000 faces:  

| Input CAD Asset | Processed Output |
|---------|-------------|
| [no.468 gt4rs.stp](<../../sample-assets/no.468 gt4rs.stp/README.md>), [Download Link](https://grabcad.com/library/porsche-718-cayman-gt4-rs-1)[<img src="../../sample-assets/no.468 gt4rs.stp/screenshot/no.468 gt4rs.jpg" width="400">](<../../sample-assets/no.468 gt4rs.stp/README.md>) | <img src="sample-results/screenshot/no.468 gt4rs.stp_decimated-500k-shaded.jpg" width="400"><img src="sample-results/screenshot/no.468 gt4rs.stp_decimated-500k-wire.jpg" width="400"> |  
<br>

Cell Phone Cover.STEP remeshed (quad-dominant) to 13,000 (= res 7) and 50,000 (= res8) triangles respectively:  

| Input CAD Asset | Processed Output |
|---------|-------------|
| [Cell Phone Cover.STEP](<../../sample-assets/Cell Phone Cover.STEP/README.md>), [Download Link](https://grabcad.com/library/cell-phone-case-4)[<img src="../../sample-assets/Cell Phone Cover.STEP/screenshot/Cell Phone Cover.STEP.jpg" width="400">](<../../sample-assets/Cell Phone Cover.STEP/README.md>) | <img src="sample-results/screenshot/Cell Phone Cover.STEP_remeshing.jpg" width="400"> |  

Note: Even though basic model representation can be achieved, [RapidPipeline's remesher](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-optimize-settings#remesher) was designed as a pre-processing step for further optimization operations (Decimation, UV unwraping, Texture Baking etc.) - for an actual "end-to-end" optimization workflow utilizing the remesher please refer to the [05_Optimization workflow section](../05_Optimization/README.md).  

<br>


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
rpdx --read_config decimation.json -i 'Cooper CAD refined.step' -r -o output  
```

#### Decimation - Combined Workflow

Decimation combined with vertex merging and winding order correction:

```
rpdx --read_config decimation_combined-workflow.json -i 'no.468 gt4rs.stp' -r -o output  
```

### Remeshing

Remeshing (quad dominant) in two different resolutions (octree level 7 = ~13k triangles with given sample asset, 8 = ~50k triangles with given sample asset) without any texture baking or material preservation as a simple capability demo:  

```
rpdx --read_config remeshing_res-7.json -i 'Cell Phone Cover.STEP' -r -o output_remeshing/res-7
```

```
rpdx --read_config remeshing_res-8.json -i 'Cell Phone Cover.STEP' -r -o output_remeshing/res-8
```

Note: Within the configuration .json settings files in this repository only `usd` output formats are specified. RapidPipeline [supports a lot more file formats](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/format-support) which can be [configured within the settings file](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7#export-slot).  

The 3D Processor CLI will automatically create a default output folder if the run command `-r` is used. In order to define a specific output path the command `-o` can be utilized instead. Read more about [file exports with the CLI here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-a-3d-file).  

Alternatively the export command `-e` can be used to generate any output path or file format.  Read more about the [export command here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-via-command).  

## Settings File

The settings file is the basis for all processing with the RapidPipeline 3D Processor CLI.  
It is following `.json` syntax and is validated against the [RapidPipeline 3D Processing Schema](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7).  

In this example we are only making use of the `Import`, `3D Edit`, `Optimize`, `Scene Graph Flattening` and `Export` sections (`objects`).  
However, there are a lot more options for 3D processing. More about combining more sections within a single settings .json file in the [Batch Processing workflow](../06_Batch-Processing/README.md).  


## Features & Settings - Main Concepts

Please see below some basic explanation for each feature and setting. Each header also contains a link to the respective sections of the RapidPipeline Documentation:  




### [Mesh Decimation - Decimator Object](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#decimator)

Simplifies a mesh by reducing the number of faces while preserving materials, UVs and textures. This is useful for reducing file size, and improving performance by reducing memory usage.  

#### [Face and Vertex Targets](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#faces-target)

The target amount (value) or percentage of faces (triangles) or vertices. RapidPipeline will attempt to get as close as possible to the chosen value, though sometimes a mesh may require a slightly different final triangle count to preserve surface continuity.

#### [Deviation Target](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#deviation-target)

When using the decimator, it is possible to specify a maximum deviation to the original surface. The deviation (= error) is the maximum distance between the two assets (original and decimated).

The deviation target is used to stop the decimation as soon as an edge collapse introduces an deviation/error larger then specified.

#### [Material Optimization Options](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#material-optimization-decimator)

Option within Decimator for optimizing the model's materials, including material merging, UV (atlas) generation, Texture Baking, and more.

There are multiple options available to pair the decimation process with:

- [Material Merger](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#material-merger-decimator)
- [Keep Materials and UVs](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#keep-materials-and-uvs-decimator)
- [Material and UV Aggregator](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#uv-aggregator-decimator)

#### [Preservation of Mesh Normals](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-optimize-settings#preserve-mesh-normals), [Topology](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-optimize-settings#preserve-topology)

Preserves vertex normals during decimation, rather than recalculating normals after merging vertices.  

Preserves topological features like holes during decimation.  


### [Remesher](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#remesher)

Remeshes the original mesh and decimates to a face or vertices target value or percentage. The initial remeshing resolution can be configured with the resolution setting.  

#### Remeshing Methods - [Voxelization](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#remesher---voxelization-method) vs [Shrinkwrap](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#remesher---shrinkwrap-method--filter-back-projected)

The voxelization method creates a more smoothed surface reconstruction which sometimes can be better for texture (normal) map baking, especially when going towards very low resolution meshes with organic features.  

The shrinkwrap method is more accurate in terms of surface reconstruction compared to the voxelization method. It is more suited for medium resolution outputs as well as for meshes with hard-surface features.  

#### [Remeshing Resolution](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#remesher)

Maximum octree depth (resolution) for the initial remeshing process before any face target is applied.  

#### [Remeshing - Face Target](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.8#faces-target-1)

The remeshing face targets perfroms and additional decimation operation after the initial remeshing.  



## Download or copy the settings file

### Decimation

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

### Decimation - Combined Workflow

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

### Remeshing

[remeshing_res-7.json](remeshing_res-7.json)

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
      }
    },
  "3dEdit": {
    "meshNormals": {
      "recomputeInputNormals": true,
      "hardAngleThreshold": 30.0,
      "computationMethod": "area"
    }
  },
  "sceneGraphFlattening": {
    "method": "byMaterial",
    "preservedSceneDepth": 0
  },
  "optimize": {
    "3dModelOptimizationMethod": {
      "meshAndMaterialOptimization": {
        "remesher": {
          "target": {
            "faces": {
              "value": 500000
            }
          },
          "materialMerger": {
            "materialRegenerator": {
              "uvAtlasGenerator": {
                "textureBaker": {
                  "normalMap": {
                    "mode": "never",
                    "recomputeNormals": false
                  },
                  "bakingResolution": {
                    "default": 0
                  }
                }
              }
            }
          },
          "method": "shrinkwrap",
          "resolution": 7,
          "filterBackProjected": true
        }
      }
    }
  },
    "export": [
        {
            "discard": {
                "emptyNodes": true, 
                "unusedUVs": true
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

[remeshing_res-8.json](remeshing_res-8.json)

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
      }
    },
  "3dEdit": {
    "meshNormals": {
      "recomputeInputNormals": true,
      "hardAngleThreshold": 30.0,
      "computationMethod": "area"
    }
  },
  "sceneGraphFlattening": {
    "method": "byMaterial",
    "preservedSceneDepth": 0
  },
  "optimize": {
    "3dModelOptimizationMethod": {
      "meshAndMaterialOptimization": {
        "remesher": {
          "target": {
            "faces": {
              "value": 500000
            }
          },
          "materialMerger": {
            "materialRegenerator": {
              "uvAtlasGenerator": {
                "textureBaker": {
                  "normalMap": {
                    "mode": "never",
                    "recomputeNormals": false
                  },
                  "bakingResolution": {
                    "default": 0
                  }
                }
              }
            }
          },
          "method": "shrinkwrap",
          "resolution": 8,
          "filterBackProjected": true
        }
      }
    }
  },
    "export": [
        {
            "discard": {
                "emptyNodes": true, 
                "unusedUVs": true
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