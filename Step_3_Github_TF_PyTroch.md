In this file, we will introduce how to set vitural environment for Tensorflow and PyTorch. There are many benefits for building separate environment for different library or deep learning environment, like it is convenient to manage packages, and much safer to test different version. Also, it's very handy to clone environment and reproduce it cross multiple platform. Hence, it's necessary to set the environment here.

# Set Virtral Environments

There are multiple ways to set virtual environments. To be more specific, I think the most commonly used ones are `venv` and `conda`. There are few different between them, and it's hard to say which one is better.

- When you set the virtual environment, `vene` requires you go to the directory where you plan to start your project, and initalize the virtual environment will generate a folder under this workspace, which reminds you this folder is work under this environment. There will be a `site-packages/` folder to store all packages installed under this environment. In short, `vene` generate the env folder in your work space. You can have multiple different env with same name as long as they are under different workspace.

- In contrast, `conda` initalizes the virtual environment under the `/Users/user-name/anaconda3/envs`. Of course it can also generate the env under specific folde path as you define. However, it will be a little bit tricky since you need to point out the path like `conda create --prefix /path/to/conda-env ` and use `conda config --set env_prompt '({name})'` to avoid long prefix problem. Also, you need to be very careful when you install packages. The recommended way to install packages is: Activate the env, then install it throght `conda install` or `pip install`. In theory both should work, the main difference is that `conda` install it through Anaconda Repository or Anaconda Cloud,but `pip` install it thorugh Pypi. The guidance says we always try `conda` first for all attribute advantages.

- We have more information in the [aterials folder.](https://github.com/ZihanChen1995/Deep-Learning-on-Ubuntu/tree/master/materials) Please check it. Also, Anaconda official website offers a great expalination on the details manipulation. Check it [here].(https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

- One more import thing. So how could we clone or copy environment? Well in `vene` they achieve it through the `requirements.txt`. This file records all the packages information that installed in this env. You can use `pip freeze > requirements.txt` for the record and `pip install -r requirements.txt` when you are in a new virtual environment. Similar to `conda`, it achieve this by `environment.yml` file. Function are all the same. You just need to `conda env export --file environment.yml`. Two things you need notice here:

* 1. Where is this `yml` file goes? 

> Well, normally it will go to the path `home/USER_NAME/` folder. In Ubuntu just open the folder and you will see it. In case you miss it, use the `sudo updatedb` first then `locate environment.yml`. You will find it.

* 2. I notice there is a absolute path under `yml` file, do I need to worry about it when I use new env?
> Actually you don't. You can just edit it and delete this line. You can find the answer [here.](https://stackoverflow.com/questions/41274007/anaconda-export-environment-file)

## Install PyTorch

As we just say, we will install PyTroch and Tensorflow under different environment. PyTroch first.

```
# creat env
conda create --name pytorch101 python=3.7

# activate it
conda activate pytorch101


# install pandas
pip install pandas

# check all env
conda env list

# check the packages under this env
conda list

# install PyTroch. (Check official website for further info)
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch

# vertify installation
python -c "import torch; print(torch.cuda.get_device_name(0))"

# close it
conda deactivate

```

## Install Tensorflow

Well many people may say Tensorflow 1 series is better. However I hold the oppsite idea for two reasons. First, we can easily achieve all functions that can be achieve in TF1 by `import tensorflow.compat.v1 as tf`. Second, TF2 combine the GPU version and CPU one. You can check the website [here](https://www.tensorflow.org/install/pip)

```
# create the env
conda create -n TF2 python=3.7

# init the env
conda activate TF2

# install tensorflow
pip install --upgrade tensorflow

# vertify the installation
python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"

# close the env
conda deactivate
```

After all these steps, you have two environments which contains PyTorch and Tensorflow.
