# Creating a custom Sagemaker image

This repo creates an docker image on AWS ECR then creates a sagemaker image

## IMAGE_NAME=geo-runtime

See [AWS repo](https://github.com/aws-samples/sagemaker-studio-custom-image-samples) for more examples.

Below is the skeleton

Coiled is used to create the conda-docker image hosted on ECR.
This requires an account on Coiled (and setting AWS as the backend)
i.e set software environments to be hosted on AWS ECR.

Coiled creates the ECR repository and the docker image for you.

1. Use coiled to create an docker image with environment file
```
import coiled
coiled.create_software_environment(
	name = "<ENV_NAME>",
	conda = "environment.yml"
	)
```
Include awscli, boto3, ipykernel in env build to for Sagemaker
		
2. Using aws cli, use the docker image in EC3 to create a sagemaker image
`aws --region ${REGION} sagemaker create-image`
    1. get sagemaker executioner role i.e. ROLE ARN from AWS console (in Sagemaker)
    2. provide the docker image in ECR as the base image
    
Sagemaker domains are defined per region (/per user) hence we need to specify region above

3. Create AppImageConfig
4. Update domain (add to image dictionary to json file)


## Notes

Coiled’s Docker file command

RUN conda env update -n coiled -f environment.yml     && rm environment.yml     && conda clean --all -y     && echo "conda activate coiled" >> ~/.bashrc


