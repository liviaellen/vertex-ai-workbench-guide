## Vertex AI Workbench

Let's explore **Vertex AI Workbench** as an alternative to **Compute Engine** for model training.

_Vertex AI Workbench_ provides Jupyter-notebook based development environments for data science. It allows you to run ML code without having to precisely configure the environment for the code. They come prepackaged with JupyterLab, NumPy, Pandas, scikit-learn, TensorFlow and PyTorch.

They also allow you to automatically run your notebook on a recurring schedule.

### Create a Workbench Instance

Create a Workbench instance:
1. Access the [Vertex AI Workbench page](https://console.cloud.google.com/vertex-ai/workbench)
1. At the top, enable the Notebooks API if it's not yet enabled. This could take a few minutes. When finished, return to the Vertex AI Workbench page.
1. At the top, select the **INSTANCES** tab and click on the blue **CREATE NEW** button further below.
1. Choose a name for your instance, for example _cloud-training-recap_, and click on the **CONTINUE** button at the bottom.
1. In the next page _Environment_, you can select the version you want to use. For now, let's keep the settings to use the latest version. Click on **CONTINUE**.
1. In the next page _Machine type_, you can see the options for your machine type. You could for example select a very powerful machine with loads of memory and GPU support. At the right you will see the estimated costs for your machine. For now, let's stick to the default machine, and click on **CONTINUE**.
1. In the next screen you could change the size of the disks if you need more disk space. Again, stick to the default.
1. We won't change the remaining settings either. So let's scroll down and click on the blue **CREATE**. (If it doesn't create, try switching to a region that has sufficient resources.)

👉 The Workbench instance should be ready in a couple of minutes.

Open the Workbench instance by clicking on **OPEN JUPYTERLAB**.

### Explore the environment

We have a complete JupyterLab environment in front of us:
- We can run a terminal
- We can run different notebooks,
  - a standard notebook for ML with numpy, pandas, and scikit-learn
  - a notebook for DL with PyTorch
  - a notebook for DL with TensorFlow

On the left, you can upload files to your instance, very much like in Google Colab. You can use it to upload your notebook, your package folders, and your data. (If your dataset is very large, it's probably better to load it directly from its source on the cloud!)

**For many use cases this will be enough for you to start working.**

The Workbench instance IDE also comes with an integrated git client. If you want to use it, checkout the documentation [here](https://cloud.google.com/vertex-ai/docs/workbench/instances/save-to-github) to set up the connection. Alternatively you can do a mini-LW setup on the Workbench instance with the instructions below.

### Use your own environment

If you are not happy with the default environment used by the Workbench instance, you can create your own.

You might want to do this if your project uses another TensorFlow version than the default Workbench instance.

To do so, you can create a new _conda_ environment by following [these instructions](https://cloud.google.com/vertex-ai/docs/workbench/instances/add-environment).

### Mini-LW setup on the Workbench instance

If you plan to use the instance for a long time, you can a mini setup to mimic the environment on your own machine and make it more comfortable to work with.

Go to the Workbench instance and open the Terminal.

#### Install zsh and oh-my-zsh

Install zsh:

``` bash
sudo apt install zsh
```

Install oh-my-zsh:

``` bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Authenticate on GitHub 1/2

Install `gh`:

```bash
sudo apt update
sudo apt install gh
```

Run the `gh auth login` command:
- Account: `GitHub.com`
- Protocol: `HTTPS`
- Authenticate Git with your GitHub credentials: `Yes`
- Authentication method: `Paste an authentication token`

#### Create a GitHub Token

Create a GitHub token to allow the workbench to access your account:
1. Access [GitHub Tokens](https://github.com/settings/tokens)
2. Click on **generate new token**
3. Fill in the **Note** field with a meaningful name, such as _Vertex AI Workbench token_
4. Check that these scopes are enabled: 'repo', 'read:org', 'workflow'
5. Click on **generate token**
6. Copy the token (you will not be able to retrieve it later)

#### Authenticate on GitHub 2/2

Paste the token in the Vertex AI instance's Terminal

#### Install `direnv`

``` bash
curl -sfL https://direnv.net/install.sh | bash
eval "$(direnv hook zsh)"
```

#### Clone your project repo

Clone your recap repo inside your Workbench instance using the `curl` command that is provided in the Kick-start terminal instructions at the top of this challenge page on Kitt.

N.B. Make sure your GCP_PROJECT environment variable is set in your .env file
``` bash
cp .env.sample .env
nano .env
```

Then run `direnv allow .`

Install your package:

``` bash
pip install -e .
make reset_local_files
```

#### Now use your package

Now that you have set up everything:
- You can **run your code** on your Workbench instance
- You can **create new notebooks to experiment**, and use the code that you already refactored into `.py` files in your package.
- Use your own machine for other things. all compute happens on the Workbench instanc:.
- **Synch your code** using `git pull` and `git push`.

You can for example run the preprocessing and the training like before:

``` bash
make run_preprocess run_train
```

## Compute Engine vs Vertex AI Workbench

In _Compute Engine_ we can see that the _Vertex AI Workbench_ uses a _Compute Engine_ instance behind the scenes:

<img src='https://wagon-public-datasets.s3.eu-west-1.amazonaws.com/data-science-images/07-ML-OPS/mlops/vertex-ai-compute-engine.png'>

The price difference between a _Compute Engine_ virtual machine and _Vertex AI Workbench_ instance is really small.

So, for your data science projects, you probably want to create a Workbench instance, and get both a notebook based interface for experimenting and a classic virtual machine to run your code.
