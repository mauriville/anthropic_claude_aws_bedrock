# anthropic_claude_aws_bedrock

Examples for calling Anthropic Claude models on AWS Bedrock from Jupyter notebooks.

## Repository structure

The notebooks are currently organized into three sections:

- `01_working_api/`
  - `001_request/`: basic Bedrock API requests
  - `002_system_promtps/`: system message examples
  - `003_streaming/`: streaming responses
  - `004_controlling_out/`: output control examples
- `02_prompt_evaluation/`
  - `001_Prompt_Evals.ipynb`
  - `001_Prompt_Evals_complete.ipynb`
- `03_prompt_engineering/`
  - `001_prompting.ipynb`
  - `002_prompting_completed.ipynb`
  - `dataset.json`

Most topics include both an in-progress notebook and a completed notebook.

## Requirements

- Python 3
- An AWS account with Bedrock enabled
- Access to the Anthropic model IDs used in the notebooks

## Create the virtual environment

From the repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install boto3 ipykernel jupyterlab
python -m ipykernel install --prefix .venv --name .venv --display-name .venv
```

## Configure AWS credentials

The notebooks use `boto3` and connect to AWS Bedrock. Before running them, configure credentials for an AWS user or role that has permission to call Bedrock.

Option 1, use `aws configure`:

```bash
aws configure
```

Example values:

```text
AWS Access Key ID: <your-access-key-id>
AWS Secret Access Key: <your-secret-access-key>
Default region name: us-west-2
Default output format: json
```

Option 2, export environment variables:

```bash
export AWS_ACCESS_KEY_ID=<your-access-key-id>
export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
export AWS_DEFAULT_REGION=us-west-2
```

If you use temporary credentials, also export:

```bash
export AWS_SESSION_TOKEN=<your-session-token>
```

Option 3, use a local `.env` file:

1. Copy the example file:

```bash
cp .env.example .env
```

2. Update `.env` with your AWS credentials:

```env
AWS_ACCESS_KEY_ID=your_access_key_id
AWS_SECRET_ACCESS_KEY=your_secret_access_key
AWS_DEFAULT_REGION=us-west-2
AWS_SESSION_TOKEN=your_session_token
```

3. Load the `.env` file into your shell before starting Jupyter:

```bash
set -a
source .env
set +a
```

This works with the current notebooks because `boto3` reads standard AWS environment variables once they are present in the shell environment.

## Run the notebooks

Start JupyterLab from the repository root:

```bash
source .venv/bin/activate
set -a
source .env
set +a
jupyter lab
```

When Jupyter opens, select the `.venv` kernel for the notebooks.

## Notes

- Do not commit your real `.env` file. This repository ignores it through `.gitignore`.
- The notebooks currently create the Bedrock client with `region_name="us-west-2"`.
- Your AWS identity must have permission for Bedrock runtime calls such as `bedrock:InvokeModel` and `bedrock:InvokeModelWithResponseStream` where applicable.
- Your account must also have model access enabled for the Anthropic models referenced in the notebooks.
