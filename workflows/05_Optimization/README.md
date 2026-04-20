# Optimization

## Description

...

### Example Files

[Game-controller-ASM.STEP](<../../sample-assets/Game-controller-ASM.STEP/README.md>)  
[Download Link](https://grabcad.com/library/xbox-style-controller)  
[<img src="../../sample-assets/Game-controller-ASM.STEP/screenshot/Game-controller-ASM.STEP.jpg" width="400">](<../../sample-assets/Game-controller-ASM.STEP/README.md>)  

## Sample Results

Game-controller-ASM.STEP remeshed to 50,000 faces:  
<img src="sample-results/screenshot/Game-controller-ASM.STEP_remeshed-50k-shaded.jpg" width="400">  
<img src="sample-results/screenshot/Game-controller-ASM.STEP_remeshed-50k-wire.jpg" width="400">  

## Steps to Reproduce

In order to reproduce the given results please follow the steps below:

### 3D Processor CLI

1. [Install and set-up the RapidPipeline 3D Processor CLI](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide)  
	- Requires RapidPipeline enterprise plan or free enterprise trial to access the CLI ([Contact here](https://rapidpipeline.com/en/contact/))  
	- Information regarding [latest version and changelog can be found here](https://docs.rapidpipeline.com/3d-processor-updates)  
2. Download the respective example file for this tutorial. You can find the files in the [overview here](../README.md).  
3. Get the respective .json settings configuration file further below and make sure input file as well as .json file are present  
4. Run the command listed below in your favorite commandline (e.g. windows powershell), more about [3D Processor CLI commands here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#commands-guide)  

Further information regarding Optimization (...) in the [RapidPipeline Documentation](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/3d-processor-overview).  

## Commands

### Remeshing & Texture Baking

```
rpdx --read_config optimize_remesh-bake.json -i 'Game-controller-ASM.STEP' -r
```

Note: Within the configuration .json settings files in this repository only `usd` output formats are specified. RapidPipeline [supports a lot more file formats](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/format-support) which can be [configured within the settings file](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7#export-slot).  

The 3D Processor CLI will automatically create a default output folder if the run command `-r` is used. In order to define a specific output path the command `-o` can be utilized instead. Read more about [file exports with the CLI here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-a-3d-file).  

Alternatively the export command `-e` can be used to generate any output path or file format.  Read more about the [export command here](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide#export-via-command).  

## Settings File

The settings file is the basis for all processing with the RapidPipeline 3D Processor CLI.  
It is following `.json` syntax and is validated against the [RapidPipeline 3D Processing Schema](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessingSchemaSettings/processor-schema-settings-v1.7).  

In this example we are only making use of the `Import`, `Optimize`, `Modifier`, `Scene Graph Flattening` and `Export` sections (`objects`).  
However, there are a lot more options for 3D processing. More about combining more sections within a single settings .json file in the [Batch Processing workflow](../06_Batch-Processing/README.md).  


## Features & Settings - Main Concepts

Please see below some basic explanation for each feature and setting. Each header also contains a link to the respective sections of the RapidPipeline Documentation:  

<!-- 
### [Mesh Culling](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings)

Various methods of culling geometry from an input model.  

### [Occlusion Culling](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/03settingsGuide/3d-processor-culling-settings#occlusion-culling)

Removes occluded (invisible) parts from the 3D model, for the whole 3D model or individually for each mesh.  

-->

### Download or copy the settings file

#### Remeshing & Texture Baking

[optimize_remesh-bake.json](optimize_remesh-bake.json)

```
{
    "import": {
      "CAD": {
        "tessellationResolution":"custom", 
        "sewTolerance": 0.01, 
        "removeTJunctions": true,
        "maxSurfaceDeviation": 0.05,
        "maxAngle": 40,
        "maxEdgeLength": 0
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
              "value": 50000
            }
          },
          "materialMerger": {
            "materialRegenerator": {
              "uvAtlasGenerator": {
                "textureBaker": {
                  "normalMap": {
                    "mode": "always",
                    "recomputeNormals": true
                  },
                  "aoBaker": {
                    "strength": 0.5,
                    "replaceOriginal": true,
                    "filterRadius": 3.0,
                    "textureSamples": 48
                  },
                  "bakingResolution": {
                    "default": 4096
                  },
                  "texMapAutoScaling": true
                },
                "atlasMode": "separateAlpha",
                "atlasFactor": 1
              }
            }
          },
          "method": "shrinkwrap",
          "resolution": 11,
          "filterBackProjected": true
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
