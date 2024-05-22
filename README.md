# Foundation Model benchmarking tool (FMBench)

<h1 align="center">
        <img src="https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/img/fmbt-small.png?raw=true"></img>
    </h1>
    <p align="center">
        <p align="center">Benchmark any Foundation Model (FM) on any AWS service [Amazon SageMaker, Amazon Bedrock, Amazon EKS, Bring your own endpoint etc.]
        <br>
    </p>
<h4 align="center"><a href="" target="_blank">Amazon SageMaker</a> | <a href="" target="_blank">Amazon Bedrock</a></h4>
<h4 align="center">
    <a href="https://pypi.org/project/fmbench/" target="_blank">
        <img src="https://img.shields.io/pypi/v/fmbench.svg" alt="PyPI Version">
    </a>    
</h4>

A key challenge with FMs is the ability to benchmark their performance in terms of inference latency, throughput and cost so as to determine which model running with what combination of the hardware and serving stack provides the best price-performance combination for a given workload.

Stated as **business problem**, the ask is “_*What is the dollar cost per transaction for a given generative AI workload that serves a given number of users while keeping the response time under a target threshold?*_”

But to really answer this question, we need to answer an **engineering question** (an optimization problem, actually) corresponding to this business problem: “*_What is the minimum number of instances N, of most cost optimal instance type T, that are needed to serve a workload W while keeping the average transaction latency under L seconds?_*”

*W: = {R transactions per-minute, average prompt token length P, average generation token length G}*

This foundation model benchmarking tool (a.k.a. `FMBench`) is a tool to answer the above engineering question and thus answer the original business question about how to get the best price performance for a given workload. Here is one of the plots generated by `FMBench` to help answer the above question (_the instance types in the legend have been blurred out on purpose, you can find them in the actual plot generated on running `FMBench`_).

![business question](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/img/business_summary.png?raw=true)

## Models benchmarked

Configuration files are available in the [configs](./src/fmbench/configs) folder for the following models in this repo.

### 🚨 Llama3 on Amazon SageMaker 🚨

Llama3 is now available on SageMaker (read [blog post](https://aws.amazon.com/blogs/machine-learning/meta-llama-3-models-are-now-available-in-amazon-sagemaker-jumpstart/)), and you can now benchmark it using `FMBench`. Here are the config files for benchmarking `Llama3-8b-instruct` and `Llama3-70b-instruct` on `ml.p4d.24xlarge`, `ml.inf2.24xlarge` and `ml.g5.12xlarge` instances.

- [Config file](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/src/fmbench/configs/llama3/8b/config-llama3-8b-instruct-g5-p4d.yml) for `Llama3-8b-instruct` on  `ml.p4d.24xlarge` and `ml.g5.12xlarge`.
- [Config file](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/src/fmbench/configs/llama3/70b/config-llama3-70b-instruct-g5-p4d.yml) for `Llama3-70b-instruct` on  `ml.p4d.24xlarge` and `ml.g5.48xlarge`.
- [Config file](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/src/fmbench/configs/llama3/8b/config-llama3-8b-inf2-g5.yml) for `Llama3-8b-instruct` on  `ml.inf2.24xlarge` and `ml.g5.12xlarge`.

### Full list of benchmarked models

| Model    | SageMaker g4dn/g5/p3 | SageMaker Inf2 | SageMaker P4 | SageMaker P5 | Bedrock On-demand throughput | Bedrock provisioned throughput |
|:------------------|:-----------------|:----------------|:--------------|:--------------|:------------------------------|:--------------------------------|
| **Anthropic Claude-3 Sonnet** | | |  | | ✅ | ✅  | 
| **Anthropic Claude-3 Haiku**  | | |  | | ✅ |   |
| **Mistral-7b-instruct** |✅ | |✅  |✅ | ✅ |   |
| **Mistral-7b-AWQ** || | |✅ | |   |
| **Mixtral-8x7b-instruct**  | | |  | | ✅ |   |
| **Llama3-8b instruct**  |✅ |✅|✅  | |✅  |   |
| **Llama3-70b instruct**  |✅ |✅|✅  | |✅ |   |
| **Llama2-13b chat**  |✅ |✅ |✅  | | ✅  |   |
| **Llama2-70b chat**  |✅ |✅ |✅  | | ✅  |   |
| **Amazon Titan text lite**  | | |  | | ✅ |   |
| **Amazon Titan text express**  | | |  | | ✅ |   |
| **Cohere Command text**  | | |  | | ✅ |   |
| **Cohere Command light text**  | | |  | | ✅ |   |
| **AI21 J2 Mid**  | | |  | | ✅ |   |
| **AI21 J2 Ultra** | | |  | | ✅ |   |
| **distilbert-base-uncased**  |  ✅ | |  | ||   |

## New in this release

### v1.0.37
1. `g5`/`p4d`/`p5`/`inf2`/`trn1` specific config files for [`Llama3-8b-Instruct`](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct).
    1. `p4d` config file for both `vllm` and `lmi-dist`.

### v1.0.36
1. Fix bug at higher concurrency levels (20 and above).
1. Support for instance count > 1.


### v1.0.35

1. Support for [Open-Orca](https://huggingface.co/datasets/Open-Orca/OpenOrca) dataset and corresponding prompts for Llama3, Llama2 and Mistral.

### v1.0.34
1. Don't delete endpoints for the bring your own endpoint case.
1. Fix bug with business summary chart.


### v1.0.32

1. Report enhancements: New business summary chart, config file embedded in the report, version numbering and others.

1. Additional config files: Meta Llama3 on Inf2, Mistral instruct with `lmi-dist` on `p4d` and `p5` instances.


## Description

`FMBench` is a Python package for running performance benchmarks for **any model** deployed on **Amazon SageMaker** or available on **Amazon Bedrock** or deployed by you on an AWS service of choice (such as Amazon EKS or Amazon EC2) a.k.a **Bring your own endpoint**. For SageMaker, `FMBench` provides both the option of deploying models on SageMaker as part of its workflow and use the endpoint or skip the deployment step and use an endpoint you provide to send inference requests to and measure metrics such as inference latency, error rate, transactions per second etc. This approach allows for benchmarking different combinations of instance types (`g5`, `p4d`, `p5`, `Inf2`), inference containers (`DeepSpeed`, `TensorRT`, `HuggingFace TGI` and others) and parameters such as tensor parallelism, rolling batch etc. Because `FMBench` is model agnostic therefore it can be used not only testing _third party models_ but also _open-source models_ and _proprietary models_ trained by enterprises on their own data.

`FMBench` can be run on any AWS platform where we can run Python, such as Amazon EC2, Amazon SageMaker, or even the AWS CloudShell. It is important to run this tool on an AWS platform so that internet round trip time does not get included in the end-to-end response time latency.

The workflow for `FMBench` is as follows:

```
Create configuration file
        |
        |-----> Deploy model on SageMaker/Use models on Bedrock/Bring your own endpoint
                    |
                    |-----> Run inference against deployed endpoint(s)
                                     |
                                     |------> Create a benchmarking report
```

1. Create a dataset of different prompt sizes and select one or more such datasets for running the tests.
    1. Currently `FMBench` supports datasets from [LongBench](https://github.com/THUDM/LongBench) and filter out individual items from the dataset based on their size in tokens (for example, prompts less than 500 tokens, between 500 to 1000 tokens and so on and so forth). Alternatively, you can download the folder from [this link](https://huggingface.co/datasets/THUDM/LongBench/resolve/main/data.zip) to load the data.

1. Deploy **any model** that is deployable on SageMaker on **any supported instance type** (`g5`, `p4d`, `Inf2`).
    1. Models could be either available via SageMaker JumpStart (list available [here](https://sagemaker.readthedocs.io/en/stable/doc_utils/pretrainedmodels.html)) as well as models not available via JumpStart but still deployable on SageMaker through the low level boto3 (Python) SDK (Bring Your  Own Script).
    1. Model deployment is completely configurable in terms of the inference container to use, environment variable to set, `setting.properties` file to provide (for inference containers such as DJL that use it) and instance type to use.

1. Benchmark FM performance in terms of inference latency, transactions per minute and dollar cost per transaction for any FM that can be deployed on SageMaker.
    1. Tests are run for each combination of the configured concurrency levels i.e. transactions (inference requests) sent to the endpoint in parallel and dataset. For example, run multiple datasets of say prompt sizes between 3000 to 4000 tokens at concurrency levels of 1, 2, 4, 6, 8 etc. so as to test how many transactions of what token length can the endpoint handle while still maintaining an acceptable level of inference latency.

1. Generate a report that compares and contrasts the performance of the model over different test configurations and stores the reports in an Amazon S3 bucket.
    1. The report is generated in the [Markdown](https://en.wikipedia.org/wiki/Markdown) format and consists of plots, tables and text that highlight the key results and provide an overall recommendation on what is the best combination of instance type and serving stack to use for the model under stack for a dataset of interest.
    1. The report is created as an artifact of reproducible research so that anyone having access to the model, instance type and serving stack can run the code and recreate the same results and report.

1. Multiple [configuration files](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/configs) that can be used as reference for benchmarking new models and instance types.

## Getting started

`FMBench` is available as a Python package on [PyPi](https://pypi.org/project/fmbench) and is run as a command line tool once it is installed. All data that includes metrics, reports and results are stored in an Amazon S3 bucket.

### Quickstart

1. Each `FMBench` run works with a configuration file that contains the information about the model, the deployment steps, and the tests to run. A typical `FMBench` workflow involves either directly using an already provided config file from the [`configs`](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/configs) folder in the `FMBench` GitHub repo or editing an already provided config file as per your own requirements (say you want to try benchmarking on a different instance type, or a different inference container etc.).

    >A simple config file with key parameters annotated is included in this repo, see [`config-llama2-7b-g5-quick.yml`](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/configs/llama2/7b/config-llama2-7b-g5-quick.yml). This file benchmarks performance of Llama2-7b on an `ml.g5.xlarge` instance and an `ml.g5.2xlarge` instance. You can use this config file as it is for this Quickstart.

1. Launch the AWS CloudFormation template included in this repository using one of the buttons from the table below. The CloudFormation template creates the following resources within your AWS account: Amazon S3 buckets, Amazon IAM role and an Amazon SageMaker Notebook with this repository cloned. A read S3 bucket is created which contains all the files (configuration files, datasets) required to run `FMBench` and a write S3 bucket is created which will hold the metrics and reports generated by `FMBench`. The CloudFormation stack takes about 5-minutes to create.

   |AWS Region                |     Link        |
   |:------------------------:|:-----------:|
   |us-east-1 (N. Virginia)    | [<img src="./img/ML-FMBT-cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=fmbench&templateURL=https://aws-blogs-artifacts-public.s3.amazonaws.com/artifacts/ML-FMBT/template.yml) |
   |us-west-2 (Oregon)    | [<img src="./img/ML-FMBT-cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=fmbench&templateURL=https://aws-blogs-artifacts-public.s3.amazonaws.com/artifacts/ML-FMBT/template.yml) |

1. Once the CloudFormation stack is created, navigate to SageMaker Notebooks and open the `fmbench-notebook`.

1. On the `fmbench-notebook` open a Terminal and run the following commands.

    ```{.bash}
    conda create --name fmbench_python311 -y python=3.11 ipykernel
    source activate fmbench_python311;
    pip install -U fmbench
    ```

1. Now you are ready to `fmbench` with the following command line. We will use a sample config file placed in the S3 bucket by the CloudFormation stack for a quick first run.
    
    1. We benchmark performance for the `Llama2-7b` model on a `ml.g5.xlarge` and a `ml.g5.2xlarge` instance type, using the `huggingface-pytorch-tgi-inference` inference container. This test would take about 30 minutes to complete and cost about $0.20.
    
    1. It uses a simple relationship of 750 words equals 1000 tokens, to get a more accurate representation of token counts use the `Llama2 tokenizer` (instructions are provided in the next section). ***It is strongly recommended that for more accurate results on token throughput you use a tokenizer specific to the model you are testing rather than the default tokenizer. See instructions provided later in this document on how to use a custom tokenizer***.

        ```{.bash}
        account=`aws sts get-caller-identity | jq .Account | tr -d '"'`
        region=`aws configure get region`
        fmbench --config-file s3://sagemaker-fmbench-read-${region}-${account}/configs/llama2/7b/config-llama2-7b-g5-quick.yml >> fmbench.log 2>&1
        ```

    1. Open another terminal window and do a `tail -f` on the `fmbench.log` file to see all the traces being generated at runtime.
    
        ```{.bash}
        tail -f fmbench.log
        ```
    
1. The generated reports and metrics are available in the `sagemaker-fmbench-write-<replace_w_your_aws_region>-<replace_w_your_aws_account_id>` bucket. The metrics and report files are also downloaded locally and in the `results` directory (created by `FMBench`) and the benchmarking report is available as a markdown file called `report.md` in the `results` directory. You can view the rendered Markdown report in the SageMaker notebook itself or download the metrics and report files to your machine for offline analysis.

### The DIY version (with gory details)

Follow the prerequisites below to set up your environment before running the code:

1. **Python 3.11**: Setup a Python 3.11 virtual environment and install `FMBench`.

    ```{.bash}
    python -m venv .fmbench
    pip install fmbench
    ```

1. **S3 buckets for test data, scripts, and results**: Create two buckets within your AWS account:

    * _Read bucket_: This bucket contains `tokenizer files`, `prompt template`, `source data` and `deployment scripts` stored in a directory structure as shown below. `FMBench` needs to have read access to this bucket.
    
        ```{.bash}
        s3://<read-bucket-name>
            ├── source_data/
            ├── source_data/<source-data-file-name>.json
            ├── prompt_template/
            ├── prompt_template/prompt_template.txt
            ├── scripts/
            ├── scripts/<deployment-script-name>.py
            ├── tokenizer/
            ├── tokenizer/tokenizer.json
            ├── tokenizer/config.json
        ```

        * The details of the bucket structure is as follows:

            1. **Source Data Directory**: Create a `source_data` directory that stores the dataset you want to benchmark with. `FMBench` uses `Q&A` datasets from the [`LongBench dataset`](https://github.com/THUDM/LongBench) or alternatively from [this link](https://huggingface.co/datasets/THUDM/LongBench/resolve/main/data.zip). _Support for bring your own dataset will be added soon_.

                * Download the different files specified in the [LongBench dataset](https://github.com/THUDM/LongBench) into the `source_data` directory. Following is a good list to get started with:

                    * `2wikimqa`
                    * `hotpotqa`
                    * `narrativeqa`
                    * `triviaqa`
                
                    Store these files in the `source_data` directory.

            1. **Prompt Template Directory**: Create a `prompt_template` directory that contains a `prompt_template.txt` file. This `.txt` file contains the prompt template that your specific model supports. `FMBench` already supports the [prompt template](src/fmbench/prompt_template/prompt_template.txt) compatible with `Llama` models.

            1. **Scripts Directory**: `FMBench` also supports a `bring your own script (BYOS)` mode for deploying models that are not natively available via SageMaker JumpStart i.e. anything not included in [this](https://sagemaker.readthedocs.io/en/stable/doc_utils/pretrainedmodels.html) list. Here are the steps to use BYOS.

                1. Create a Python script to deploy your model on a SageMaker endpoint. This script needs to have a `deploy` function that [`2_deploy_model.ipynb`](./src/fmbench/2_deploy_model.ipynb) can invoke. See [`p4d_hf_tgi.py`](./scripts/p4d_hf_tgi.py) for reference.

                1. Place your deployment script in the `scripts` directory in your ***read bucket***. If your script deploys a model directly from HuggingFace and needs to have access to a HuggingFace auth token, then create a file called `hf_token.txt` and put the auth token in that file. The [`.gitignore`](.gitgnore) file in this repo has rules to not commit the `hf_token.txt` to the repo. Today, `FMBench` provides inference scripts for:

                    * [All SageMaker Jumpstart Models](https://docs.aws.amazon.com/sagemaker/latest/dg/jumpstart-foundation-models.html)
                    * [Text-Generation-Inference (TGI) container supported models](https://huggingface.co/text-generation-inference)
                    * [Deep Java Library DeepSpeed container supported models](https://docs.djl.ai/docs/serving/serving/docs/lmi/configurations_large_model_inference_containers.html)


                    Deployment scripts for the options above are available in the [scripts](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/s3_metrics/scripts) directory, you can use these as reference for creating your own deployment scripts as well.

            1. **Tokenizer Directory**: Place the `tokenizer.json`, `config.json` and any other files required for your model's tokenizer in the `tokenizer` directory. The tokenizer for your model should be compatible with the [`tokenizers`](https://pypi.org/project/tokenizers/) package. `FMBench` uses `AutoTokenizer.from_pretrained` to load the tokenizer.
                >As an example, to use the `Llama 2 Tokenizer` for counting prompt and generation tokens for the `Llama 2` family of models: Accept the License here: [meta approval form](https://ai.meta.com/resources/models-and-libraries/llama-downloads/) and download the `tokenizer.json` and `config.json` files from [Hugging Face website](https://huggingface.co/meta-llama/Llama-2-7b/tree/main) and place them in the `tokenizer` directory.

    * _Write bucket_: All prompt payloads, model endpoint and metrics generated by `FMBench` are stored in this bucket. `FMBench` requires write permissions to store the results in this bucket. No directory structure needs to be pre-created in this bucket, everything is created by `FMBench` at runtime.

        ```{.bash}
        s3://<write-bucket-name>
            ├── <test-name>
            ├── <test-name>/data
            ├── <test-name>/data/metrics
            ├── <test-name>/data/models
            ├── <test-name>/data/prompts
        ````

### Bring your own `Rest Predictor` ([`data-on-eks`](https://github.com/awslabs/data-on-eks/tree/7173cd98c9be6f555afc42f8311cc7849f74a038) version)

`FMBench` now provides an example of bringing your own endpoint as a `REST Predictor` for benchmarking. View this [`script`](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/REST-predictor-fmbench/src/fmbench/scripts/rest_predictor.py) as an example. This script is an inference file for the `NousResearch/Llama-2-13b-chat-hf` model deployed on an [Amazon EKS](https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/amazon-elastic-kubernetes-service.html) cluster using [Ray Serve](https://docs.ray.io/en/latest/ray-overview/examples.html). The model is deployed via `data-on-eks` which is a comprehensive resource for scaling your data and machine learning workloads on Amazon EKS and unlocking the power of Gen AI. Using `data-on-eks`, you can harness the capabilities of AWS Trainium, AWS Inferentia and NVIDIA GPUs to scale and optimize your Gen AI workloads and benchmark those models on FMBench with ease. 


### Bring your own `dataset` | `endpoint`

`FMBench` started out as supporting only SageMaker endpoints and while that is still true as far as _deploying_ the endpoint through `FMBench` is concerned but we now support the ability to support external endpoints and external datasets.

#### Bring your own dataset

By default `FMBench` uses the [`LongBench dataset`](https://github.com/THUDM/LongBench) dataset for testing the models, but this is not the only dataset you can test with. You may want to test with other datasets available on HuggingFace or use your own datasets for testing. You can do this by converting your dataset to the [`JSON lines`](https://jsonlines.org/) format. We provide a code sample for converting any HuggingFace dataset into JSON lines format and uploading it to the S3 bucket used by `FMBench` in the [`bring_your_own_dataset`](./src/fmbench/bring_your_own_dataset.ipynb) notebook. Follow the steps described in the notebook to bring your own dataset for testing with `FMBench`.

##### Support for Open-Orca dataset

Support for [Open-Orca](https://huggingface.co/datasets/Open-Orca/OpenOrca) dataset and corresponding prompts for Llama3, Llama2 and Mistral, see:

1. [bring_your_own_dataset.ipynb](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/bring_your_own_dataset.ipynb)
1. [prompt templates](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/prompt_template)
1. [Llama3 config file with OpenOrca](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/configs/llama3/8b/config-llama3-8b-inf2-g5-byoe-w-openorca.yml)

#### Bring your own endpoint (a.k.a. support for external endpoints)

If you have an endpoint deployed on say `Amazon EKS` or `Amazon EC2` or have your models hosted on a fully-managed service such as `Amazon Bedrock`, you can still bring your endpoint to `FMBench` and run tests against your endpoint. To do this you need to do the following:

1. Create a derived class from [`FMBenchPredictor`](./src/fmbench/scripts/fmbench_predictor.py) abstract class and provide implementation for the constructor, the `get_predictions` method and the `endpoint_name` property. See [`SageMakerPredictor`](./src/fmbench/scripts/sagemaker_predictor.py) for an example. Save this file locally as say `my_custom_predictor.py`.

1. Upload your new Python file (`my_custom_predictor.py`) for your custom FMBench predictor to your `FMBench` read bucket and the scripts prefix specified in the `s3_read_data` section (`read_bucket` and `scripts_prefix`).

1. Edit the configuration file you are using for your `FMBench` for the following:
    - Skip the deployment step by setting the `2_deploy_model.ipynb` step under `run_steps` to `no`.
    - Set the `inference_script` under any experiment in the `experiments` section for which you want to use your new custom inference script to point to your new Python file (`my_custom_predictor.py`) that contains your custom predictor.

### Steps to run

1. `pip install` the `FMBench` package from PyPi.

1. Create a config file using one of the config files available [here](https://github.com/aws-samples/foundation-model-benchmarking-tool/tree/main/src/fmbench/configs).
    1. The configuration file is a YAML file containing configuration for all steps of the benchmarking process. It is recommended to create a copy of an existing config file and tweak it as necessary to create a new one for your experiment.

1. Create the read and write buckets as mentioned in the prerequisites section. Mention the respective directories for your read and write buckets within the config files.

1. Run the `FMBench` tool from the command line.

    ```{.bash}
    # the config file path could be an S3 path and https path 
    # or even a path to a file on the local filesystem
    fmbench --config-file \path\to\config\file
    ```

1. Depending upon the experiments in the config file, the `FMBench` run may take a few minutes to several hours. Once the run completes, you can find the report and metrics in the write S3 bucket set in the [config file](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/src/fmbench/configs/config-mistral-7b-tgi-g5.yml#L12). The report is generated as a markdown file called `report.md` and is available in the metrics directory in the write S3 bucket.

## Results

Here is a screenshot of the `report.md` file generated by `FMBench`.
![Report](https://github.com/aws-samples/foundation-model-benchmarking-tool/blob/main/img/results.gif?raw=true)


## Building the `FMBench` Python package

The following steps describe how to build the `FMBench` Python package.

1. Clone the `FMBench` repo from GitHub.

1. Make any code changes as needed.

1. Install [`poetry`](https://pypi.org/project/poetry/).
   
    ```{.bash}
    pip install poetry
    ```

1. Change directory to the `FMBench` repo directory and run poetry build.

    ```{.bash}
    poetry build
    ```

1. The `.whl` file is generated in the `dist` folder. Install the `.whl` in your current Python environment.

    ```{.bash}
    pip install dist/fmbench-X.Y.Z-py3-none-any.whl
    ```

1. Run `FMBench` as usual through the `FMBench` CLI command.

## Pending enhancements

View the [ISSUES](https://github.com/aws-samples/foundation-model-benchmarking-tool/issues) on GitHub and add any you might think be an beneficial iteration to this benchmarking harness.

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the [LICENSE](./LICENSE) file.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=aws-samples/foundation-model-benchmarking-tool&type=Date)](https://star-history.com/#aws-samples/foundation-model-benchmarking-tool&Date)

[![Stargazers repo roster for @aws-samples/foundation-model-benchmarking-tool](https://reporoster.com/stars/aws-samples/foundation-model-benchmarking-tool)](https://github.com/aws-samples/foundation-model-benchmarking-tool/stargazers)
