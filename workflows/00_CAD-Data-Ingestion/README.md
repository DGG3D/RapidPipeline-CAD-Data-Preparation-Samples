# Title

## Description

...

## Steps to Reproduce

### 3D Processor CLI

1. [Install and set-up the RapidPipeline 3D Processor CLI](https://docs.rapidpipeline.com/docs/componentDocs/3dProcessor/04cliDocumentation/cli-setup-guide)  
	- Requires RapidPipeline enterprise plan or free enterprise trial to access the CLI ([Contact here](https://rapidpipeline.com/en/contact/))  
2. Download the respective example file for this tutorial. You can find the files in the [overview here](../README.md).  
3. Get the respective .json settings configuration file further below and make sure input file as well as .json file are present  
4. Run the command listed below in your favorite commandline (e.g. windows powershell)

### Example Files

[EG 43-17 HG Pojemnik.STEP](<../../sample-assets/EG 43-17 HG Pojemnik.STEP/README.md>)  
[Download Link](https://grabcad.com/library/eg43-17-hg-1)  
<img src="../../sample-assets/EG 43-17 HG Pojemnik.STEP/screenshot/EG 43-17 HG Pojemnik.jpg" width="400">  

[no.468 gt4rs.stp](<../../sample-assets/no.468 gt4rs.stp/README.md>)  
[Download Link](https://grabcad.com/library/porsche-718-cayman-gt4-rs-1)  
<img src="../../sample-assets/no.468 gt4rs.stp/screenshot/no.468 gt4rs.jpg" width="400">  

### Commands

```
rpdx -i 
```

#### Settings File

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
