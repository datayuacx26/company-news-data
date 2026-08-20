---
schema_version: "1.0.0"
document_id: "17851affba72d28367c05aebd9c81a7c56f2357e687c0a30d39e4897ba102c98"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/ultimate-guide-to-huggingfacehub-library-in-python"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:010653881566a0098d2dd5307930abf3e6ec17f1939b2873e02b51d81327b3ce"
---

# Ultimate guide to huggingface_hub library in Python

[← Back to all posts](https://deepnote.com/blog)


The huggingface_hub library is a Python package that provides a seamless interface to the Hugging Face Hub, enabling developers to share, download, and manage machine learning models, datasets, and other artifacts in a centralized way.


It serves as the official client for interacting with Hugging Face’s platform, streamlining tasks such as model distribution, versioning, and collaboration across the AI community. Huggingface_hub was created by the Hugging Face team (including core members like Julien Chaumond and Lysandre Debut) to simplify the process of integrating Hugging Face’s repository of models into Python workflows. Since its introduction in late 2020, the library has become a cornerstone of the Python machine learning ecosystem, widely used alongside libraries like Transformers and datasets.


Historically, Hugging Face launched this library to fill a growing need for a unified way to access and share ML models. Prior to **huggingface_hub** , developers often had to manually download model files or use specialized scripts; the library emerged as a response to the community’s desire for a more convenient solution. Huggingface_hub’s development was driven by Hugging Face, Inc., reflecting the company’s mission to democratize AI by making model sharing as easy as importing a Python module. The initial releases focused on basic features like file downloads and repository management, but over time the library has expanded significantly. It has continuously evolved through an open-source development model, with contributions from Hugging Face’s engineers and community members, ensuring it stays relevant to emerging needs.


In the Python ecosystem, **huggingface_hub** occupies a critical position as the go-to library for model and dataset management. It underpins many popular frameworks: for example, the Transformers library uses huggingface_hub under the hood to fetch pre-trained models, and the Datasets library uses it to load community-contributed datasets. This means that when you call` transformers.AutoModel.from_pretrained()` , it’s actually the huggingface_hub client doing the heavy lifting of downloading weights and caching them. The library’s main use cases include programmatically downloading model files, uploading new model versions or entire repositories, performing searches on the Hub, and even running cloud-based model inference. It serves researchers publishing new models, data scientists collaborating on model versions, and engineers building ML pipelines that need to retrieve or push models reliably.


For Python developers, learning **huggingface_hub** is increasingly important. With over 750,000 public models hosted on Hugging Face Hub as of mid-2024 (and the number growing rapidly), the ability to easily pull these resources into your projects can significantly speed up development. Instead of writing custom code to handle model downloads, authentication, and version control, developers can rely on this library’s well-tested functions. This not only accelerates prototyping and research (by quickly accessing state-of-the-art models) but also facilitates reproducibility and collaboration. Projects that use huggingface_hub can share their models with a single command, making it straightforward for others to reproduce results or fine-tune existing models. In an era where open-source models and datasets drive innovation, being proficient with huggingface_hub is a valuable skill that opens doors to a vast ecosystem of AI resources.


The **huggingface_hub** library is actively maintained and up-to-date. At the time of writing, the latest stable release is version **0.34.4** (August 2025), with new versions being released frequently. The maintainers (listed on PyPI as Hugging Face team members) ensure compatibility with new features of the Hugging Face Hub and address community-reported issues. The project is open-source under the Apache 2.0 license, encouraging contributions and transparency. Despite not yet reaching a 1.0 version, huggingface_hub is considered production-ready and is used in numerous real-world applications. Its development trajectory indicates a commitment to backward compatibility (major breaking changes are rare and usually tied to major version milestones) and continuous improvement – for example, the library adopted support for faster file storage backends (like **hf_xet** for large files) and an improved CLI (` huggingface-cli` now accessible as` hf` ) as the ecosystem grew. Overall, huggingface_hub’s current status is that of a mature, well-supported library that is essential for anyone working with Hugging Face’s machine learning platform.


## What is huggingface_hub in Python?


In technical terms, **huggingface_hub** is a Python SDK (Software Development Kit) that enables direct interaction with the Hugging Face Hub – a collection of git-based repositories for machine learning models, datasets, and applications (Spaces). At its core, huggingface_hub abstracts away the details of communicating with the Hub’s REST APIs and git repositories, providing high-level Python functions and classes to perform common tasks. Key concepts include *repositories* (each model or dataset is a repo on the Hub), *files and revisions* (each repo can have multiple files and versions, tracked by git commits or tags), and *authentication tokens* (used to manage private content and publish updates). Essentially, huggingface_hub can be thought of as a bridge between your local Python environment and the Hugging Face cloud, handling everything from file transfers to authentication under the hood.


The library’s architecture blends two paradigms: a traditional Git-based approach and an HTTP-based API approach. Initially, huggingface_hub was built around a **Repository** class that wraps git commands to clone and push to Hub repos, which allowed users to manipulate repositories similarly to using Git locally. This approach ensures that entire repositories (with all files and history) can be managed on disk. However, it required users to have Git installed and handle large files via Git LFS. As the library evolved, a more flexible **HfApi** class (HTTP-based) was introduced, allowing direct HTTP calls to the Hub for operations like downloading a single file, creating a repo, or adding a file – all without needing a full git clone. Under the hood, huggingface_hub now favors this HTTP approach for most tasks, as it’s lighter weight and doesn’t require maintaining a local repo copy. The` Repository` class is still available (for those who prefer or need git semantics) but is marked deprecated and is expected to be removed by version 1.0. Instead, HfApi and its helper functions drive most features, making the library operate more like a typical REST client with clever caching.


Huggingface_hub is organized into several key modules and components, each responsible for different functionality. The **file download** module provides functions like` hf_hub_download` and` snapshot_download` for fetching files or entire repositories efficiently. There’s an **upload** module with functions such as` upload_file` and` upload_folder` , and corresponding low-level API methods that handle HTTP` PUT` requests to upload content to the Hub. The **search** capabilities are exposed via methods like` HfApi().search_models()` or` list_models()` , which query the Hub for repositories matching certain criteria. Authentication is handled by either environment variables (like` HF_TOKEN` ) or an interactive login utility (the library provides a CLI command` hf auth login` as well as Python methods to login or pass tokens). Another important component is the **inference** client – huggingface_hub includes an` InferenceClient` that can call remote inference endpoints or services (such as Hugging Face’s hosted inference API or third-party providers) to run predictions using models on the Hub. This means you can perform tasks like text generation or image classification through the Hub’s infrastructure directly from Python, without loading the model locally. Internally, huggingface_hub also manages **caching** (storing downloaded files under a cache directory with content hashing for versioning) and **error handling** (defining exceptions like` RepositoryNotFoundError` or` HfHubHTTPError` for common error scenarios). All these components work together to present a cohesive interface that hides the complexity of cloud storage, HTTP requests, and git operations, so that developers can work with Hub content as if it were local files.


One of the strengths of **huggingface_hub** is how it integrates with other Python libraries and frameworks. It has been designed to be framework-agnostic: whether you’re using PyTorch, TensorFlow, JAX, or just plain Python, you can use huggingface_hub to fetch model weights or data and then load them into your library of choice. For example, if you prefer TensorFlow, you can use huggingface_hub to download a SavedModel or Keras H5 file from the Hub and then load it with` tf.keras.models.load_model` . Similarly, PyTorch users can download a` .bin` or` .safetensors` weights file and load it into a model class with` torch.load` or via Transformers. The library also has specialized integration guides – Hugging Face provides documentation on how to combine huggingface_hub with libraries like *fastai* , *spaCy* , or *Gradio* , showing patterns for using the Hub to publish models or assets from those frameworks. As an example, spaCy has a` spacy-huggingface-hub` plugin that uses huggingface_hub under the hood to push spaCy pipelines to the Hub. Furthermore, huggingface_hub’s **CLI** (invoked by the` hf` command) is built on the library, which means any operations you do via the command line (like logging in, uploading a model, or listing repos) are actually using the same Python code. This allows for consistency – you can prototype something on the command line and then automate it in a Python script with minimal changes. The architecture thus promotes a unified experience: whether through direct Python calls, CLI usage, or being invoked inside higher-level libraries, huggingface_hub acts as the central engine driving interactions with the Hugging Face Hub.


In terms of performance and behavior, huggingface_hub is designed to be efficient and robust for large-scale model handling. File downloads are automatically cached: when you download a model or dataset file, it’s stored in a local cache (` ~/.cache/huggingface/hub` by default), keyed by a unique hash or etag. If you request the same file again (even in a different session or program), the library will skip the network call and load it from cache, unless you explicitly force a download or a newer version is available. This caching mechanism greatly improves performance for iterative development, where you might load models repeatedly. Huggingface_hub also supports **concurrent downloads** for repositories: the` snapshot_download` function, for instance, can download multiple files in parallel using up to 8 threads by default. This was an improvement added to speed up grabbing entire model repos with many files (especially useful for large models or datasets with hundreds of files). Memory-wise, the library streams data to files, so downloading even very large weights (multiple GBs) won’t blow up your RAM usage – it writes to disk chunk by chunk. It also emits progress bars (via the` tqdm` library) by default for long downloads, which can be disabled if needed. Another performance feature is the integration of **Xet storage** for large files: starting from version 0.32.0, huggingface_hub uses a backend called Xet (a chunked storage system) for new repositories, which results in faster downloads and uploads for huge files due to deduplication and optimized transfers. From the user perspective, this is mostly transparent, but it means that updating large models is more efficient (only changed parts of files are sent, saving bandwidth). Overall, huggingface_hub’s under-the-hood working is geared towards reliability (retries on failed requests, informative exceptions), compatibility (works across OSes, with special handling for Windows symlink issues), and efficiency (caching, parallelism, and incremental updates). This allows developers to trust the library in production scenarios where robustness and performance are as important as ease of use.


## Why do we use the huggingface_hub library in Python?


One of the main reasons developers use **huggingface_hub** is to solve the otherwise tedious problem of managing machine learning models and datasets across different environments. Without this library, sharing a model might involve manually uploading files to cloud storage or a custom server, writing code to download those files, handling version mismatches, and so on. Huggingface_hub eliminates those hurdles by providing a one-stop solution: with a single` pip install` and a few lines of code, you can download a model from a central repository or publish your own for others to use. It addresses the *specific problem* of model distribution by abstracting away the storage details – you don’t need to know if a model is stored on S3, Cloudfront, or elsewhere because huggingface_hub figures that out for you and delivers the file. This is particularly useful in research and industry settings where reproducibility is key: using the library, you can programmatically ensure you’re getting the exact version of a model (via commit hashes or version tags) required for your project, avoiding the “it works on my machine” syndrome. Additionally, huggingface_hub handles large file pointers and git LFS details for you, meaning you can work with huge models (gigabytes in size) without dealing with the intricacies of large file storage. In summary, it solves the distribution, versioning, and storage headaches that come with modern ML assets.


Another benefit of huggingface_hub is the **performance advantage** it offers through caching and optimized transfers. In practice, using this library is often faster and more reliable than manually downloading files via direct URLs or other methods. When you use` huggingface_hub.hf_hub_download` , for example, the library will automatically direct your request to a CDN (Content Delivery Network) URL if available, which can significantly speed up downloads by fetching from a server geographically close to you. It also uses efficient HTTP requests under the hood, including range requests for partial downloads if needed. The caching mechanism means that repeated runs of your code won’t repeatedly hit the network, which not only improves speed but also reduces load on the server side. For large repositories, the introduction of multi-threaded downloads means you can saturate your bandwidth and cut waiting times (imagine pulling down a dataset of 1000 images – huggingface_hub can fetch many files in parallel, completing the task much faster than a one-by-one approach). The library also manages **resume** for downloads – if a download is interrupted, the cache system can detect a partially downloaded file and resume it next time, sparing you from re-downloading everything. In terms of efficiency gains, huggingface_hub can turn what might be a manual multi-step process (find link, download, unzip, move files, etc.) into a quick function call. This allows developers to focus on model development or analysis instead of the mundane details of file handling. Additionally, because huggingface_hub is well-integrated, it avoids many common errors that can slow you down – for instance, it will automatically retry on transient network failures, and it verifies file integrity via checksums (ETags) to ensure the downloaded content is exactly what’s expected. All these performance considerations make it a robust choice for production pipelines where time and reliability are paramount.


Using the **huggingface_hub** library also significantly improves development *efficiency* . For developers, time is saved not only through raw performance but through easier workflows. Want to try a new model for sentiment analysis? Instead of searching the web for a download link, figuring out how to load it, and worrying about dependencies, you can do` model_path = hf_hub_download(repo_id="distilbert-base-uncased-finetuned-sst-2-english", filename="pytorch_model.bin")` and then load the model weights directly. The library’s integration with Hugging Face’s **Model Hub** means you have programmatic access to a vast catalogue of pre-trained models, organized by task, architecture, dataset, etc. This accelerates experimentation: a single script can pull in a language model, a vision model, and a dataset from the Hub, all using the same API. Moreover, huggingface_hub allows for quick iteration in collaborative settings. If you fine-tune a model, you can share it by using` upload_file` or` create_repo` and your teammate can fetch it immediately – no emails with attachments, no file transfer services required. The standardization that huggingface_hub brings (everyone using the same commands to share and load models) reduces the friction in team environments and open-source projects. It essentially acts as a *package manager for models* ; similar to how` pip` made it trivial to share and reuse code libraries, huggingface_hub makes it trivial to share and reuse trained models and datasets. This leads to development efficiency gains because you’re building on top of proven components rather than reinventing the wheel.


Industry adoption of huggingface_hub underscores its real-world applicability. Many companies and research labs use Hugging Face Hub as their model repository, and huggingface_hub is the tool that bridges their internal workflows with that repository. For example, a machine learning team might train models on a private server or cloud environment; using huggingface_hub, they can push each trained version to a private repository on the Hub as part of their CI/CD pipeline. This serves as an **MLOps** solution for version control of models – every model version gets a git commit and can be tagged, compared, or rolled back if needed. Real-world applications range from **NLP** (sharing fine-tuned BERT models for classification) to **computer vision** (distributing pre-trained object detectors or diffusion models) and even **audio** or **multimodal** models. The library has been used in producing demos (like powering Hugging Face Spaces where an app automatically loads a model via huggingface_hub and runs inference) and in production services (such as loading a model on a server for an API without bundling the model weights in the application code). The wide adoption also means there’s strong community support – many common issues have known solutions, and best practices have emerged (which are reflected in this guide’s later sections). Importantly, huggingface_hub lets you **compare doing tasks with vs. without** the library: doing them without often involves a lot of boilerplate and potential for error, whereas with huggingface_hub, tasks become one-liners. For example, consider uploading a model without huggingface_hub – you’d have to manually handle authentication, possibly use Git or a POST API call, handle failures, etc. With huggingface_hub, you can simply call` api.upload_file(path_or_fileobj="model.bin", path_in_repo="model.bin", repo_id="username/my-model")` and the library deals with everything else, including retries and checking for success. This frees up developer energy to focus on higher-level problems like model accuracy and user experience.


Finally, using **huggingface_hub** is vital for enabling a culture of collaboration and open science in the ML community. By lowering the barrier to sharing models, it encourages practitioners to publish their results and make them accessible. This is apparent when comparing doing a project without huggingface_hub (where a model might remain on a local disk, effectively siloed) versus with huggingface_hub (where publishing is as easy as a function call, and the model can immediately reach thousands of other developers). The library’s ease of use has directly contributed to the explosive growth of models on the Hugging Face Hub – instead of ML results being locked away in papers or behind company doors, more and more are released with code and weights via this tool. For developers, this means a richer set of building blocks to start from. You rarely have to start from scratch on a new problem; chances are someone has shared a model or dataset that gives you a head start, and huggingface_hub lets you incorporate that in minutes. This collaborative multiplier effect is something intangible but very real: it can drastically reduce project timelines and spur innovation. In summary, huggingface_hub is used because it makes the **impossible** (or at least the very difficult) *possible* , and the difficult *easy* , when it comes to managing the lifeblood of AI projects – the models and data.


## Getting started with huggingface_hub


### Installation instructions


Installing **huggingface_hub** is straightforward and supports multiple methods, depending on your environment and preferences. The library requires Python 3.8 or above, so ensure your Python version meets that requirement first. Below are various installation methods for local development setups:


-


**Using pip (PyPI):** The simplest way is via pip. Open a terminal or command prompt and run:


```text
pip install huggingface_hub
```


This will download and install the latest release from the Python Package Index. If you already have it installed but want to upgrade, run` pip install --upgrade huggingface_hub` to get the newest version. It’s recommended to do this within a virtual environment (such as using` python -m venv .env` and activating it) to avoid conflicts with other packages. After installation, you can verify it by starting a Python shell and importing the package:


```text
import huggingface_hub  print(huggingface_hub.__version__)
```


This should display the version number if installation was successful. If you encounter a permission error on Unix-based OS (Linux/Mac) when using` pip install` , you might need to prepend` --user` or use a virtualenv. On Windows, if not using a virtual environment, running the command in an Administrator Command Prompt or using the` py -m pip install ...` syntax can help.


-


**Using conda (Anaconda/Miniconda):** If you prefer the Conda package manager, huggingface_hub can be installed from the conda-forge channel. First ensure you have conda installed, then run:


```text
conda install -c conda-forge huggingface_hub
```


This will fetch the package (and any required dependencies) from conda-forge. Using conda is convenient in Anaconda Navigator or Miniconda environments. In **Anaconda Navigator** , you can go to the **Environments** tab, search for “huggingface_hub” (make sure to select Channels to include conda-forge), and install it via the GUI. In **Miniconda** or **Conda CLI** , the above command suffices. If the package is not found, ensure that the conda-forge channel is added (` conda config --add channels conda-forge` ). The conda installation is functionally similar to pip. After installation, you can test it by activating the environment (` conda activate <env_name>` ) and importing the library in Python.


-


**In Visual Studio Code (VS Code):** VS Code itself doesn’t install packages, but you can use its integrated terminal or the Python extension’s interface. Open the integrated terminal in VS Code (` Ctrl+\`` or through the menu) and run the pip or conda command as described above. If you have the Python extension, you can also open the **Command Palette** and select “Python: Create Terminal” which opens a terminal with the correct environment. Then use` pip install huggingface_hub` . Alternatively, if you’re using a **requirements.txt** or **poetry/conda** environment, add` huggingface_hub` to those and let VS Code (with extensions like PyPI Manager or the built-in dependency manager) handle the installation. After installing, VS Code should recognize the package in IntelliSense; you may need to reload the window or ensure the correct interpreter is selected if the package isn’t being detected.


-


**In PyCharm:** PyCharm provides a UI to install packages into the project’s interpreter. You can go to **File > Settings > Project: <ProjectName> > Python Interpreter** , then click the “+” button to add a new package. Search for “huggingface_hub” in the dialog, select the latest version, and install it. PyCharm will handle the installation and show progress. Alternatively, you can use PyCharm’s terminal (at the bottom of the IDE) and run the pip install command. If you’re using a PyCharm-managed virtual environment, ensure you’ve activated it or use the PyCharm terminal which does it automatically. Once installed, PyCharm should be able to import huggingface_hub in your code without issues. If PyCharm doesn’t recognize the package after installation, double-check that you installed it to the correct interpreter (PyCharm might have multiple interpreters if you use different run configurations).


-


**Installation on different operating systems:**


-


*Windows:* Use the Command Prompt or PowerShell to run` pip install huggingface_hub` . On Windows, you might use` py -m pip install huggingface_hub` to specify the Python interpreter. If using Conda on Windows, open the “Anaconda Prompt” and run the conda install command. One specific consideration for Windows is that huggingface_hub may use symbolic links for caching by default; if you encounter a warning about Developer Mode or needing admin privileges (for symlinks), you can enable Windows Developer Mode or run Python as administrator to allow symlinks (this improves caching efficiency). This is not an installation blocker, just a post-install note.


-


*macOS:* Use the Terminal app. Usually` pip3 install huggingface_hub` (since macOS often has Python 3 as` pip3` ) will install it. On newer macOS versions with Apple Silicon, pip will install an appropriate wheel (the library is pure Python, so no special binaries needed). If using Homebrew’s Python, ensure you use the corresponding pip. Conda on macOS works via the same` conda install -c conda-forge huggingface_hub` .


-


*Linux:* Use your distro’s terminal. With pip, the command is the same. If you’re on a Debian/Ubuntu system and prefer apt (not common for Python packages), note that huggingface_hub is likely not available via apt; pip or conda is recommended. Linux users should also ensure` pip` is updated (` pip install --upgrade pip` ) if any SSL or connection issues occur during installation.


-


**Using Docker:** If you are containerizing your application, you can install huggingface_hub in your Dockerfile. For example, if using the official Python base image, your Dockerfile might include:


```text
FROM python:3.11-slim    RUN pip install --no-cache-dir huggingface_hub
```


This will install huggingface_hub inside the container. You can also add it to a` requirements.txt` and use` pip install -r requirements.txt` in the Docker build. The` --no-cache-dir` option is optional but recommended in Docker to avoid caching wheels and reduce image size. Ensure your Docker image has internet access during build to download the package. If working in an environment like Docker Compose, simply rebuild the image after adding huggingface_hub to your dependencies. Once installed in a container, usage is identical to local usage.


-


**Virtual environments:** It’s a best practice to install huggingface_hub inside a virtual environment to avoid dependency conflicts. If using` venv` or` virtualenv` , activate your environment (` source venv/bin/activate` on Linux/Mac,` .\\venv\\Scripts\\activate` on Windows) then run the pip install command. For **Poetry** users, you can add huggingface_hub as a dependency (` poetry add huggingface_hub` ) and Poetry will handle creating the venv and installing it. For **Pipenv** , use` pipenv install huggingface_hub` . In all cases, the library has relatively minimal dependencies (like` requests` ,` tqdm` , etc.), so it won’t bloat your environment. Using a venv also helps if you need to install a specific version of huggingface_hub for compatibility – you can do` pip install huggingface_hub==0.25.0` (for example) in that isolated environment without affecting your system Python or other projects.


-


**Installation in cloud or remote environments:** If you’re on a remote server (for instance an EC2 instance, or a remote development VM), the process remains the same. SSH into your server and use pip or conda to install. Make sure that the user account has permission to install packages (if not, consider using a virtual environment or the` --user` flag for pip). If internet access is restricted, you might need to download the wheel on a machine with access and transfer it, or configure proxy settings for pip. In generic cloud environments (not referring to specific platforms), huggingface_hub can be installed as long as Python is present. Some cloud ML platforms provide notebooks or managed environments – in those, usually` !pip install huggingface_hub` in a notebook cell would install it. (We avoid mentioning specific platforms like Colab or SageMaker per the restrictions, but the general approach applies: use their package manager in whatever interface is provided.)


**Troubleshooting installation:**


During installation, a few common issues may arise. If you get a` ModuleNotFoundError` for` huggingface_hub` even after installing, double-check that you’re running Python in the same environment where you installed the package. In IDEs or notebooks, sometimes the kernel or interpreter is different – ensure they match. If using conda and you see an error like *No module named 'huggingface_hub.utils'* after installing transformers, it might indicate that transformers was installed without the hub dependency. The fix is to explicitly install huggingface_hub (e.g.,` conda install -c conda-forge huggingface_hub` , as one Stack Overflow user suggested). Version mismatches can also cause errors – for example, if a library expects a newer function from huggingface_hub that isn’t present, upgrade huggingface_hub; conversely, if a library hasn’t been updated to a breaking change in the latest huggingface_hub, you may need to downgrade huggingface_hub to a compatible version (for instance, LlamaIndex required huggingface_hub 0.24.0 when a newer version removed a module it used). In summary, installation is typically quick and easy, and the multi-platform support (pip, conda) means you can get started with huggingface_hub in virtually any development setup.


### Your first huggingface_hub example


Let’s walk through a simple example to familiarize you with using **huggingface_hub** . In this example, we will programmatically search the Hugging Face Hub for a model and then download a file from that model’s repository. We’ll use a public sentiment analysis model as our target (for instance, a distilled BERT model fine-tuned on SST-2 for sentiment classification). The code will demonstrate searching, downloading, and reading the model’s configuration file, with explanatory comments for each step. This example will be beginner-friendly and self-contained, showing you how to set up authentication if needed and handle errors gracefully.


```text
# Import necessary classes from huggingface_hub from huggingface_hub import HfApi, hf_hub_download   # We will search for a model by name or description. Let's define a search query:  model_query = "sentiment analysis bert" # Use the HfApi to search for models related to the query  api = HfApi()  try:      results = api.search_models(query=model_query, limit=3)  except Exception as e:      print(f"Search failed: {e}")      results = []   # Print out the top results if results:      print("Top models for query:", model_query)      for model in results:          # Each result is a ModelInfo object with attributes like modelId, description, etc. print(f"- {model.modelId} (downloads: {model.downloads})")  else:      print("No models found for the query.")   # For this example, let's pick a specific known model repo if available or fallback:  target_repo = "distilbert-base-uncased-finetuned-sst-2-english" if not any(model.modelId == target_repo for model in results):      # If our target isn't in the search results, we'll proceed with it explicitly print(f"Proceeding with default model: {target_repo}")   # Now let's download a specific file from this model's repository, e.g., the config file  file_name = "config.json" try:      config_path = hf_hub_download(repo_id=target_repo, filename=file_name)      print(f"Downloaded {file_name} to local path: {config_path}")  except Exception as e:      print(f"Failed to download {file_name}: {e}")      config_path = None # If download was successful, open and inspect a bit of the file if config_path:      try:          with open(config_path, 'r') as f:              config_content = f.read(500)  # read first 500 characters print(f"Contents of {file_name} (truncated):\n{config_content}...")      except Exception as e:          print(f"Error reading the downloaded file: {e}")
```


**Line-by-line explanation:**


-


We start by importing` HfApi` and` hf_hub_download` from huggingface_hub.` HfApi` is a class that provides methods to interact with the Hub (like searching, listing models, creating repos, etc.), and` hf_hub_download` is a convenience function to directly download a file from a repository.


-


We define a` model_query` string for what we want to search. In this case,` "sentiment analysis bert"` – this is a general query and the Hub’s search will use it to find relevant models (the search considers model names, tags, and descriptions).


-


We instantiate` api = HfApi()` . This object will be used to call the Hub APIs. By default, this will work for public models without any authentication. If we needed to access a private model or do something requiring login, we would use` api.set_access_token("YOUR_TOKEN")` or login via` huggingface-cli` beforehand.


-


We then call` api.search_models(query=model_query, limit=3)` . This queries the Hub and returns at most 3 results. We wrap this in a try/except to handle potential exceptions (for example, if there’s a network issue or the Hub is unreachable, an exception like` requests.HTTPError` could be thrown). If an exception occurs, we print an error and proceed with` results = \[\]` to avoid using an undefined variable.


-


If the search is successful,` results` will be a list of` ModelInfo` objects. We print out a header and then loop through the results. For each model result, we print the model’s identifier (` model.modelId` , which is in the format "username/model_name") and the number of downloads (` model.downloads` ) as an example of available info. This loop will list the top models that match our query.


-


We set` target_repo` to a known model ID` "distilbert-base-uncased-finetuned-sst-2-english"` . This is a well-known sentiment model on the Hub. In a real scenario, you might pick one of the search results (for example,` results\[0\].modelId` if you trust the top result). Here, for reproducibility, we choose a specific model. We then check if that model was in our search results. If not, we print that we’re proceeding with the default model anyway.


-


Next, we want to download a file from the chosen model repository. We choose` config.json` , which is typically present in Transformer model repos and contains the model configuration (architecture, hyperparameters, etc.). We call` hf_hub_download` with` repo_id=target_repo` and` filename=file_name` . We don’t specify a` cache_dir` or` revision` , so it will download the latest version of that file to the default cache location. We again wrap this in try/except. If it fails (e.g., if the repo or file doesn’t exist, or network issues), we print a failure message and keep` config_path = None` . On success,` hf_hub_download` returns the local file path where the file is stored (usually something like` ~/.cache/huggingface/hub/models--distilbert-base-.../snapshots/<commit_id>/config.json` ).


-


If the download succeeded, we print the path for confirmation. Then we proceed to open the file and read a portion of it. We do this inside another try/except to handle any file I/O errors (though none are expected since the file should be there). We read the first 500 characters of the JSON config and print them out, truncated, to give a taste of the content. Typically, the config.json will include things like` "hidden_size": 768, "num_attention_heads": 12, ...` etc. We print this to show that we indeed have the file content.


-


Each operation is accompanied by error handling. We handle search errors, download errors, and file read errors separately, printing informative messages. This is good practice, especially with network operations, because things can fail and it’s better to catch exceptions than to have the script crash without explanation.


**Expected output:**


When you run this script, you should see something like:


```text
Top models for query: sentiment analysis bert    - distilbert-base-uncased-finetuned-sst-2-english (downloads: 123456)    - nlptown/bert-base-multilingual-uncased-sentiment (downloads: 98765)    - ... (possibly another model)   Proceeding with default model: distilbert-base-uncased-finetuned-sst-2-english    Downloaded config.json to local path: /home/youruser/.cache/huggingface/hub/models--distilbert-base-uncased-finetuned-sst-2-english/snapshots/xxxxxxxx/config.json    Contents of config.json (truncated):    {    "_name_or_path": "distilbert-base-uncased",    "architectures": ["DistilBertForSequenceClassification"],    "attention_probs_dropout_prob": 0.1,    "hidden_act": "gelu",    ... (more JSON content) ...
```


The actual numbers of downloads and content will vary, but the pattern will be similar. The script first lists some model(s) that match the query. Then it confirms which model it’s using. It downloads the config.json and prints the path. Finally, it prints the beginning of the JSON content, confirming that we successfully retrieved data from the Hub.


**Common beginner mistakes to avoid:**


-


Not being logged in when trying to access a private or gated model. If the model requires authentication (e.g., it’s a private repository or has a user agreement),` hf_hub_download` will throw a 401 or 403 error. Beginners might not realize this. The solution is to use` huggingface-cli login` in the terminal or set up` api.set_access_token("token")` before downloading. Public models (like the one in our example) don’t need this.


-


Misusing the` repo_id` format. It should be` "username/repo_name"` for user repositories or` "organization/repo_name"` for organization repositories. A common mistake is to just put the model name without the username (for community models this won’t resolve correctly) or to include the entire URL. You should pass just the ID, not a URL (i.e., use` "huggingface-course/bert-finetuned-sst2"` not` "https://huggingface.co/huggingface-course/bert-finetuned-sst2"` ). In our example, we correctly used the repo ID string.


-


Forgetting to specify` filename` when using` hf_hub_download` . If you call it without a filename, it doesn’t know what to download and will error out. Always provide a valid file name that exists in the repo (you can find file names by browsing the model’s page on the Hub or using the` HfApi().list_repo_files()` method).


-


Ignoring exceptions entirely. Beginners might be tempted to assume the download will always work. In practice, network issues or typos can occur. Our example demonstrates catching exceptions and providing feedback. Without this, a beginner might be confused if nothing happens or if the script crashes due to an uncaught error.


-


Another mistake could be not understanding where the file was downloaded. We printed` config_path` to show the actual path. If you don’t capture the return value of` hf_hub_download` , you might not know where the file went. It goes to the cache by default, which is fine to use, but as a beginner you might expect it in the working directory. Knowing the path helps if you want to copy it or just understand caching.


-


Lastly, some beginners might try to download a whole model (like the weights file) and load it without the proper library. For instance, downloading a PyTorch` .bin` model file is fine, but you then need to use Transformers or torch to actually load that file into a model class. Huggingface_hub doesn’t automatically instantiate the model (since it doesn’t assume which framework you’re using). In our example, we stuck to reading a config file which is straightforward. If you were to download a model weight file next, remember you’d use the appropriate library (Transformers, Keras, etc.) to load it.


By stepping through this example, you’ve performed a typical workflow: search the Hub, pick a model, download a file, and inspect it. This only scratches the surface of huggingface_hub’s capabilities, but it’s a solid start that shows the basic interaction pattern. Next, we’ll dive deeper into the core features of the library and explore more complex examples.


## Core features of huggingface_hub


*(In this section, we will cover key features of the huggingface_hub library. Each feature will have its own subsection with explanations, usage examples, and tips. The major features we’ll explore are: (1) Downloading files and models from the Hub, (2) Repository creation and file uploads, (3) Searching for models and datasets, (4) Running inference through the Hub, and (5) Community and collaboration utilities. These represent a broad range of what huggingface_hub can do. Each subsection will delve into why the feature is important, how to use it (with syntax and parameters explained), multiple practical examples, performance considerations, integration notes, and common errors/solutions.)*


### Downloading files and models from the Hub


**What it does and why it’s important:** Downloading files is perhaps the most fundamental feature of the huggingface_hub library. It allows you to retrieve model weights, configuration files, tokenizer files, dataset splits, and any other artifact stored in a Hugging Face Hub repository, all from within your Python code. This is crucial because it turns the Hub into an extension of your local environment – you can think of it like importing a package, except instead of code, you’re importing data or models. Without huggingface_hub, you might have to manually find URLs or use git to clone repos to get these files. The library’s download functions simplify this to one-liners and handle caching, versioning, and consistency for you. For instance, if you need a pre-trained model for your NLP task,` huggingface_hub` lets you grab the model’s files with minimal effort, which is both time-saving and less error-prone. Downloads are version-aware, meaning you can specify exact model versions (via git commit hashes or tags) to ensure reproducibility. This feature is also important for building applications that dynamically load models – for example, a web app that loads different models on demand based on user input can use huggingface_hub to fetch the needed model files at runtime.


**Syntax and parameters:** The primary functions for downloading are` hf_hub_download` and` snapshot_download` .


-


` hf_hub_download(repo_id, filename, revision=None, cache_dir=None, local_dir=None, **kwargs)` will download a single file from the specified repo. Key parameters:


-


` repo_id` (str): The repository identifier in the format "namespace/repo_name". This can be a user or organization name followed by the repo name.


-


` filename` (str): The name of the file in the repository you want to download (e.g., "pytorch_model.bin", "config.json").


-


` revision` (str, optional): Which version of the repo to download from. This could be a branch name (like "main"), a tag (like "v1.0"), or a commit hash. If omitted, it defaults to the latest revision (usually the head of the default branch).


-


` cache_dir` (str, optional): By default, files go to the standard HF cache. You can override this to a custom directory if you want all files stored somewhere specific.


-


` local_dir` (str, optional): Instead of using the cache, you can specify a directory to place the downloaded file. If` local_dir` is used, huggingface_hub will replicate the repo structure in that local directory and put the file there.


-


Other optional parameters include things like` token` (if you need to pass an auth token explicitly),` force_download` (to bypass cache even if file is present), and` local_dir_use_symlinks` (which controls whether to use symlinks in local_dir mode, default is 'auto' which will use symlinks if possible).


-


The return value is the local file path of the downloaded file.


-


` snapshot_download(repo_id, revision=None, repo_type=None, ... , max_workers=8, **kwargs)` downloads an entire repository at a given revision as a snapshot on your local disk. Key parameters:


-


` repo_id` (str): same as above, which repo to get.


-


` revision` (str, optional): which commit/branch/tag to snapshot. If not provided, defaults to latest.


-


` repo_type` (str, optional): if you want something other than a model repo, you can specify "dataset" or "space". For models you usually don’t need to set this (defaults to model).


-


` max_workers` (int, optional): number of threads for parallel download. Default is 8 threads, meaning up to 8 files will download concurrently. You can adjust this (setting to 1 forces sequential download, or higher to attempt more concurrency).


-


It also supports` cache_dir` and` local_dir` similarly. When you snapshot, it will create a directory structure identical to the repo (including all files and subfolders).


-


Return value is the path to the local directory of the snapshot.


Other related functions:


-


` HfApi().list_repo_files(repo_id, revision=None)` – returns a list of file names in the repo (which can be helpful to know what files are available to download).


-


` HfApi().download_file(repo_id, filename, revision=None, local_dir=None)` – a similar single-file download method (internally,` hf_hub_download` is often easier to use, but this exists for completeness or specific use cases).


-


The library also provides lower-level functions in case needed, like constructing URLs (` hf_hub_url` ), but most users will stick to the high-level ones above.


**Practical examples:**


1.


*Downloading a model’s weight file:* Suppose you want to fine-tune a model or use it in a custom framework that doesn’t automatically integrate with the Hub. For example, you want the weights of` bert-base-uncased` . You could do:


```text
from huggingface_hub import hf_hub_download  weights_path = hf_hub_download(repo_id="bert-base-uncased", filename="pytorch_model.bin")
```


This will download the 400+ MB PyTorch weight file for BERT (if not already cached) and return the path. You can then load these weights in PyTorch:


```text
import torch  state_dict = torch.load(weights_path, map_location="cpu")  # Now you have the weights, which you could load into a model architecture manually if needed.
```


If you prefer the TensorFlow version:


```text
tf_weights = hf_hub_download(repo_id="bert-base-uncased", filename="tf_model.h5")
```


to get the Keras H5 weight file for the same model.


2.


*Downloading a specific version of a file:* The Hub’s repositories can evolve. If you want a specific version, use the` revision` parameter. For example:


```text
config_old = hf_hub_download(repo_id="bert-base-uncased", filename="config.json", revision="v1")
```


If` "v1"` is a tag in that repo pointing to an older commit, this gets the config.json as it was in version 1. You could similarly use a commit SHA if you have it, e.g.,` revision="7391afc..."` . This ensures you have *exactly* the file corresponding to that model version, which is important for experiments or deployments that require consistent setup. Using tags like this is analogous to checking out a git tag.


3.


*Downloading a dataset file:* huggingface_hub is not just for models. For instance, to download the CSV of a dataset on the Hub:


```text
csv_path = hf_hub_download(repo_id="glue", repo_type="dataset", filename="glue/sst2/train.csv")
```


Here we set` repo_type="dataset"` because "glue" is a dataset repository, not a model. The` filename` is the path within the dataset repo to the file we want (some datasets have nested directories). This call would fetch the training CSV for the SST-2 task from the GLUE dataset. You could then load it into pandas:


```text
import pandas as pd  df = pd.read_csv(csv_path)  print(df.head())
```


One thing to note: for datasets with multiple configuration (like multiple splits or subsets), the files might be organized in subfolders. The` list_repo_files` method can help figure out the correct file paths to use in such cases.


4.


*Using snapshot_download for an entire repo:* If you want everything from a model (e.g., all weight files, tokenizer, config, README, etc.), you can do:


```text
from huggingface_hub import snapshot_download  local_dir = snapshot_download("bert-base-uncased")
```


This will create a local folder (likely under the cache, something like` ~/.cache/huggingface/hub/models--bert-base-uncased--snapshots--<hash>` ) containing all files from that repo at the latest revision. You can specify` local_dir="my_bert_base_uncased"` if you want them in a specific directory of your choosing. After this, you could treat that folder like a local clone of the model repo – for example, point your model loader to that directory if it supports it:


```text
from transformers import BertModel  model = BertModel.from_pretrained(local_dir)  # loading from local snapshot
```


This avoids repeated downloads if you plan to reuse the model multiple times or want to examine all the files offline.


5.


*Partial downloads with allow_patterns:* For very large repositories (like some datasets with thousands of files), you might not want to download everything.` snapshot_download` has` allow_patterns` and` ignore_patterns` to filter files. For example, if you had a repo with images:


```text
snapshot_download("username/huge_image_repo", allow_patterns="*.jpg")
```


This would download only files ending in .jpg from the repo, ignoring others. Conversely, you could ignore large files by pattern. This is a more advanced usage but is extremely useful for performance if you know exactly what subset of files you need.


**Performance considerations:** Downloading can be network-intensive, so a few tips:


-


**Caching is your friend:** By default, huggingface_hub will not re-download a file if it’s already in your cache. If you suspect the file might be updated and you want the latest, you could either specify a new revision or use` force_download=True` . But generally, trust the caching – it saves time and bandwidth. The cache location can be configured via environment variable` HF_HOME` or` HF_HUB_CACHE` if needed.


-


**Concurrent downloads:** As noted,` snapshot_download` will use up to 8 threads by default. If you’re on a very high-bandwidth server or need faster, you could bump` max_workers` . However, beyond a certain point, you might hit diminishing returns or even strain the server. Eight is a sensible default for many cases. If you find your network is the bottleneck, increasing threads won’t help much (but if latency per file is the issue, concurrency helps).


-


**Large files (Gigabytes):** The library can handle them, but ensure you have enough disk space in the cache location. If you encounter warnings about disk space or partial downloads, check available storage. Also, on Windows, if symlinks are disabled, large files might consume double space due to the fallback mechanism (copying rather than linking caches). Enabling symlinks (Developer Mode) on Windows can mitigate that.


-


**Resume and retries:** huggingface_hub is built to handle interruptions. If you cancel a download halfway, the partially downloaded file stays in cache with a temp name. On the next call, it will resume where it left off rather than starting over. Likewise, transient network errors result in automatic retries (the underlying requests library plus some built-in retry logic in huggingface_hub). If you are downloading huge models (like 10+ GB models), using` snapshot_download` might be safer because it’s designed to handle multi-file robustly. For a single large file,` hf_hub_download` plus the resume logic should suffice.


-


**Local filesystem speed:** When downloading thousands of small files, the bottleneck might actually be writing to disk repeatedly. The concurrency helps, but also consider that after downloading, scanning through those files (like loading each image) will involve disk read overhead. Using a dataset library (like` datasets` ) that streams from the cache or using the` allow_patterns` to only get what you need can save time. In other words, try not to download 100k files if you only need 10k of them.


-


**Keep an eye on memory:** Usually not a problem because huggingface_hub streams to disk. But if you use` local_dir` without symlinks, it might duplicate some data in memory (especially if local_dir is on a different drive and it has to copy from cache). This is an edge case. By default, memory usage should be low.


-


**Multi-process consideration:** If you have multiple processes or jobs downloading the same files concurrently (say you launched two scripts that both call` hf_hub_download` for the same big file at the same time), they might both attempt to download and potentially corrupt the cache. The library uses file locks to mitigate this in many cases, but it’s something to be aware of. Best practice: you can call` snapshot_download` once in a setup stage, then have all processes read from that local directory rather than each doing their own download.


**Integration examples:** The download feature integrates with other libraries in the sense that once a file is downloaded, you can load it with the appropriate tool. Some examples:


-


With **Transformers** library: Instead of doing` AutoModel.from_pretrained("model_name")` which internally calls huggingface_hub, you might do` local_dir = snapshot_download("model_name")` and then` AutoModel.from_pretrained(local_dir)` . This can be useful if you want to ensure the model is available offline or inspect it first.


-


With **TensorFlow/Keras** : After downloading a model’s` .h5` or SavedModel directory, use` tf.keras.models.load_model(local_path)` or` TFAutoModel.from_pretrained(local_dir)` .


-


With **PyTorch** : After` hf_hub_download` to get a` .bin` file, load it via` torch.load` , then set` model.load_state_dict(state_dict)` .


-


With **datasets** library: Actually, the` datasets` library uses huggingface_hub internally for loading dataset scripts and data. But if you wanted to manually download part of a dataset and then use pandas or numpy, you could do so as shown earlier with CSVs.


-


With **OpenCV or PIL** : If you downloaded images (say via snapshot_download), you can then use OpenCV to read them from the local directory.


-


The integration is mostly about using the output of huggingface_hub’s download in whatever framework expects a file path or bytes. Because huggingface_hub gives you actual file paths, almost any library can consume those (e.g., loading a tokenizer with` tokenizer = AutoTokenizer.from_pretrained(local_dir)` after snapshotting a model repo that contains tokenizer files).


**Common errors and solutions:**


-


*File not found (404 error):* If you get an error indicating a file or repo is not found (e.g.,` RepositoryNotFoundError` or` HTTPError 404` ), double-check the repo_id and filename. Spelling matters, and filenames are case-sensitive on the Hub. Also ensure you’re using the correct` repo_type` if it’s not a model. If the repo is private or gated, a 404 might actually be returned instead of 403 for unauthorized; in that case, you need to authenticate.


-


*Authentication error (401/403):* This happens when trying to download from a private model or a model that requires acceptance of terms. The solution is to login (` huggingface-cli login` in a shell, which will save your token for future runs) or to provide a token via the` token=` argument in` hf_hub_download` . If terms need to be accepted (like some large models require agreeing to a license on the website), you must do that through the Hugging Face website with your account first – once accepted, your token will gain access.


-


*SSL certificate error:* Occasionally, users in restrictive networks (corporate environments) might encounter SSL issues. This is not specific to huggingface_hub but the underlying` requests` library. A common Stack Overflow suggestion is to ensure` certifi` is up-to-date or to set` requests` to trust your corporate root cert. As a workaround (not recommended generally), one might disable SSL verification, but it’s better to fix the cert trust. If you see an error like “SSLError: \[SSL: CERTIFICATE_VERIFY_FAILED\]”, this is the kind of issue. Installing certifi or using an environment variable like` REQUESTS_CA_BUNDLE` to point to a company CA bundle can resolve it.


-


*Timeouts:* If a download hangs or times out (could happen on very slow connections or if the file is enormous and the default timeout is exceeded), you can tweak some settings. The` hf_hub_download` doesn’t expose a direct timeout parameter, but you could modify the` HfApi` session or use` requests` environment variables. Alternatively, splitting the content (if possible) or manually downloading with resume (the library should handle resume if re-invoked).


-


*Cache inconsistency:* In rare cases, if your cache gets corrupted (maybe from an interrupted download that didn’t register properly), you might get errors reading the file. Deleting the cache entry (you can remove the folder under` ~/.cache/huggingface/hub` corresponding to that model or file) and re-downloading usually fixes this. The library tries to use etags to ensure integrity, but nothing is foolproof.


-


*“Not enough disk space” warnings:* The library will warn if it thinks the file is too large for available disk space[kb.databricks.com](https://kb.databricks.com/machine-learning/not-enough-disk-space-error-when-downloading-a-model-from-hugging-face#:~:text=Not%20enough%20disk%20space%20error,The%20expected%20file%20size) . If you know you have space in another drive, set` HF_HOME` to a path on that drive to relocate the cache there. Or free up space accordingly.


-


*Operating system quirks:* On Windows, as mentioned, symlink warnings might appear. These are warnings, not fatal. The library will continue but use a slower method. If you’re a Windows user who frequently downloads large models, consider enabling Developer Mode or running your script as admin at least once to allow symlinks creation, or set the environment variable` HF_HUB_DISABLE_SYMLINKS_WARNING=1` to suppress if you don’t care and accept the performance hit.


-


*Using an outdated version of huggingface_hub:* If you see errors like` ImportError: cannot import name 'hf_hub_download'` or missing functions, ensure you have a recent version. For instance, early versions had a function` cached_download` which is now replaced by` hf_hub_download` ; code examples online might use old names. Update the library (` pip install -U huggingface_hub` ) to get the latest API. Similarly, if` snapshot_download` didn’t exist in very old versions, upgrading will provide it.


-


*File permission issues:* If you’re running in an environment where the default cache directory is not writable (say a readonly filesystem or a container with limited permissions), you can specify` cache_dir` to somewhere writable. Alternatively, use` local_dir` to direct output to a known good location. The cache directory is typically in your home directory, which in most cases is writable, but on some shared clusters, one might mount a drive for caches.


Downloading files is a gateway to most things you’ll do with huggingface_hub. Once you’re comfortable with` hf_hub_download` and` snapshot_download` , you can leverage a huge repository of resources easily. Next, we’ll look at how to go the other way: uploading files and managing your own repositories on the Hub.


### Repository management and file uploads


**What it does and why it’s important:** Beyond just consuming content, huggingface_hub allows you to **create and update repositories on the Hub** , meaning you can programmatically share your models, datasets, or other files. This feature is what turns the Hub into a two-way collaboration platform rather than a static model zoo. By using huggingface_hub’s repository management features, you can automate the process of publishing new model versions (for example, after training completes, your code could directly push the model to the Hub). It’s important for continuous integration/continuous deployment (CI/CD) setups, as well as for individuals who want a convenient way to upload without leaving their Python environment. Instead of manually using git or the web interface, you can script everything: repository creation, adding files, deleting or moving files, and even committing changes with messages. This is particularly helpful when sharing large models or complex repositories – huggingface_hub handles chunking large files, uses your authentication tokens, and provides a consistent API for both models and datasets. Essentially, repository management features let you use the Hub as a remote storage and version control for your ML artifacts directly from code.


**Syntax and all parameters:**


The main ways to manage repos are:


-


High-level functions:` create_repo` ,` upload_file` ,` upload_folder` ,` delete_file` , etc., which are available via direct import from` huggingface_hub` .


-


Using the` HfApi` methods:` HfApi().create_repo` ,` HfApi().upload_file` , etc., which under the hood do the same thing but allow more configuration if needed (like setting repo visibility, etc.).


-


Using the` Repository` class for git-based operations (less used now, but still available).


Key functions and their parameters:


-


` create_repo(repo_id, token=None, organization=None, repo_type=None, private=False, exist_ok=False)` :


-


` repo_id` can be a simple name for a repo (like "my-model") or a full name "username/my-model". If you provide just a name and have an` organization` specified, it will create under that org; otherwise under your user.


-


` organization` (str, optional): The name of the org under which to create the repo (if you want it in an org instead of your user).


-


` repo_type` (str, optional): "model", "dataset", or "space". Defaults to "model" if not specified.


-


` private` (bool): whether to create it as a private repo (only accessible with your token or those you share with).


-


` exist_ok` (bool): if False, will raise an error if a repo with that name exists. If True, will not error (useful for idempotent operations where you don't care if it exists).


-


` token` (str, optional): an auth token if you are not logged in or want to specify a different account’s token.


-


Returns some info about the created repo (or basically confirms creation). Often you don’t even need the return; if it doesn’t error, the repo is made.


Example:` create_repo("new-model", private=True)` would create a private model repo under your username called "new-model".


-


` upload_file(path_or_fileobj, path_in_repo, repo_id, repo_type="model", revision="main", token=None, commit_message=None, commit_description=None, run_as_future=False)` : This will upload a single file to the specified repo.


-


` path_or_fileobj` : can be a local file path or a file-like object (like an open file or BytesIO).


-


` path_in_repo` : the destination path/name within the repo (e.g., "pytorch_model.bin" or "folder/data.csv").


-


` repo_id` : which repo to upload to (e.g., "username/repo-name").


-


` revision` : by default "main". If you want to upload to a different branch, you can specify the branch name. If the branch doesn’t exist, the library will create it.


-


` commit_message` and` commit_description` : you can specify a custom commit message for this upload. If not provided, a default like "Upload file to repo" is used.


-


` run_as_future` : advanced use; if True, it returns a Future object (for async uploading, seldom used in simple scripts).


-


It will raise exceptions if something goes wrong (like auth or nonexistent repo). On success, it might return some metadata or simply not throw an error.


Example:` upload_file("local/path/model.bin", path_in_repo="model.bin", repo_id="username/new-model", commit_message="Add model weights")` .


-


` upload_folder(folder_path, repo_id, path_in_repo="", **kwargs)` : Uploads all files from a local folder to the repo, optionally under a specified directory in the repo.


-


` folder_path` : path to the local directory you want to upload.


-


` path_in_repo` : a subdirectory in the repo to place these files (if "", uploads to root of repo).


-


Other params mirror those of upload_file (token, commit message, etc.).


-


This is convenient when you have a model saved in a directory (like a SavedModel or a model with multiple files) – you can push the whole thing in one call.


-


Note: It will upload all files recursively. If the folder has a lot of files, it will iterate them all. Large files are handled but will take time. The function is not atomic per file; it uploads one file at a time under the hood, making multiple commit operations or a single commit with multiple files depending on how it’s implemented currently.


-


` HfApi().delete_file(path_in_repo, repo_id, revision="main", token=None, commit_message=None)` : Delete a file from the repo at given revision (likely main).


-


Useful if you want to remove or replace files.


-


If you delete and then upload a file with the same name in separate operations, that could be two commits. Usually, if replacing, one can just upload (which will overwrite the file content in a new commit).


-


` HfApi().list_files_info` or` list_repo_files` : These can retrieve what’s currently in the repo, which is helpful to decide if you need to upload or skip something.


-


` Repository` class (from` huggingface_hub import Repository` ): Allows you to clone a repo locally and use git commands (` repo.git_add()` ,` repo.git_commit()` ,` repo.git_push()` ). This is an alternative approach – you actually get a local git repo. For large models it may not be ideal to clone everything. But for some use cases (like working with git for diff and merges) it’s available. The` Repository` class has parameters like` clone_from` (the repo_id) and can auto-track the Large File Storage (LFS) pointers. Note that huggingface_hub’s` Repository` automatically sets up Git LFS tracking for certain file patterns (like .bin, .zip, etc.), so if you add a large file through it, it will use LFS by default.


**3-4 practical examples (simple to advanced):**


1.


*Creating a repository and uploading a model file:* Suppose you just trained a model and saved` pytorch_model.bin` and` config.json` locally. You want to share it on the Hub.


```text
from huggingface_hub import create_repo, upload_file  repo_id = "your-username/my-awesome-model"  create_repo(repo_id, private=False, exist_ok=True)  # Now upload files  upload_file(path_or_fileobj="path/to/pytorch_model.bin",              path_in_repo="pytorch_model.bin",              repo_id=repo_id,              commit_message="Upload model weights")  upload_file(path_or_fileobj="path/to/config.json",              path_in_repo="config.json",              repo_id=repo_id,              commit_message="Upload model config")  print("Model files uploaded to", repo_id)
```


In this example, we set` exist_ok=True` to avoid an error if we run it again (so it won’t complain the repo exists). We upload two files with appropriate commit messages. Each` upload_file` here triggers an HTTP PUT request to the Hub. The library groups file uploads into as few commits as possible; since we called them separately, they might be two commits. We could also batch them using` upload_folder` or use` commit_message="Add config and weights"` on one of them and list multiple files, but the simpler approach is fine. After running this, the repo` your-username/my-awesome-model` will have the two files and show two commits in the Hub interface with our messages. Note: if` pytorch_model.bin` is large (over 50MB), huggingface_hub will automatically handle it via LFS so that the Hub stores it efficiently. The user doesn’t need to do anything special for that – it’s built-in.


2.


*Uploading a whole model folder at once:* Often, after using a Transformer’s` save_pretrained` or a similar method, you have a directory with multiple files (weights, config, tokenizer, etc.). Instead of uploading each manually, use` upload_folder` :


```text
from huggingface_hub import upload_folder  local_model_dir = "./my_bert_model" # contains config.json, pytorch_model.bin, tokenizer.json, etc.  upload_folder(folder_path=local_model_dir,                path_in_repo="",                repo_id="your-username/my-bert",                commit_message="Initial model upload")
```


This will iterate through all files in` my_bert_model` and upload them under the root of the repo (because path_in_repo is ""). If your local folder has subdirectories, those subdirectories will be created in the repo accordingly. The commit message will be applied to the entire batch. Under the hood, huggingface_hub might upload sequentially but group it logically as one commit (depending on version – historically it might have done multiple, but the newer implementations aim to reduce commit count).


Advanced note:` upload_folder` doesn’t automatically delete files in the repo that are not in the local folder. It’s not a full sync, just an upload. If the repo already had some files with same names, those will be overwritten by new commit versions.


3.


*Updating a file (new model version):* Let’s say you fine-tuned your model further and now have an updated` pytorch_model.bin` . You can upload it again:


```text
upload_file("new_checkpoint.bin", path_in_repo="pytorch_model.bin",              repo_id="your-username/my-awesome-model",              commit_message="Update model weights after further fine-tuning")
```


This will create a new commit in the repo where` pytorch_model.bin` is replaced by the new file. The commit message will show up in the Hub. The config and other files remain unchanged. Anyone pulling the model’s` main` branch now gets the new weights, and the old weights are still accessible in the git history if needed (or via the commit hash referencing the older commit). The library ensures large file changes are handled (if using LFS, it might store a new pointer and new blob in S3).


Performance tip: uploading a ~1GB model might take some time, but huggingface_hub uses multi-part uploads for large files to make it robust. It will show a progress bar in console by default for large files being uploaded.


4.


*Working with branches for safe updates:* Suppose you want to add a new file but not immediately to main. You can do:


```text
create_branch = HfApi().create_repo(repo_id="your-username/my-awesome-model", revision="experiment", exist_ok=True)  # Actually, `create_repo` doesn’t directly create branches, we need:  HfApi().create_branch(repo_id="your-username/my-awesome-model", branch="experiment", token=<your_token>)  # Pseudocode: huggingface_hub might not have a direct create_branch in older versions, but you can push to a new revision name.  upload_file("test_results.txt", path_in_repo="test_results.txt",              repo_id="your-username/my-awesome-model", revision="experiment",              commit_message="Add evaluation results on experiment branch")
```


This will create or use an "experiment" branch separate from main and commit the new file there. This is advanced usage – typically if you want to share intermediate results or do a PR-like workflow, huggingface_hub supports it. Later you could merge branches via the Hub UI or` HfApi().merge_request` if that existed (not sure if library supports making PRs yet, but one can always manually go to UI for merging).


5.


*Deleting or moving files:* If you realize you uploaded something sensitive or large by mistake, you can remove it:


```text
from huggingface_hub import delete_file  delete_file(path_in_repo="debug.log", repo_id="your-username/my-awesome-model", commit_message="Remove debug log")
```


This will delete` debug.log` from the main branch. Under the hood, it’s a commit that removes the file (so technically it’s still in git history but no longer in the latest version). If the file was large, it’s still in the LFS history but not listed, and if you need it completely gone for security, you might have to contact Hugging Face support to purge it (just a note: deletion via commit is not a secure erase of history, but for general cleanup it’s fine).


**Performance considerations (uploads):**


-


**Large files and LFS:** Huggingface_hub automatically uses a special upload mechanism for files above certain size (5MB threshold for LFS by default). It splits them and uploads in parts. This is usually efficient, but note that uploading a multi-GB file will still depend on your upload bandwidth. If you have slow internet, it can take a long time. There’s no direct resume for uploads in the Python client if interrupted (unlike downloads which resume), so if an upload fails halfway (due to a connection drop), you might have to rerun it. However, the library will not commit a half file – it either completes or errors out.


-


**Batching commits:** If you have many small files to upload, doing them one by one (with separate commit messages) will create many commits, which might clutter history and is slower (each commit has overhead of hitting the API, etc.). It’s faster to use` upload_folder` or group changes in one commit. The huggingface_hub library as of v0.8 onward supports specifying a list of files to upload in one commit via` upload_file` by calling it multiple times with` run_as_future=True` and then waiting, or by using` repository` and doing one commit after adding all. But simplest is` upload_folder` or just minimizing separate calls.


-


**Parallelism:** The library doesn’t currently upload multiple files in parallel (each call handles its file fully). If you needed to speed up uploading many files and you have a high-bandwidth connection, you might script using threads or futures. For example, if you have 100 small files, you could spawn threads each calling` upload_file` . The huggingface_hub is thread-safe if each call has its own file, but be cautious with rate limits. Typically it’s not needed, as network is usually the limit, not the overhead.


-


**Rate limiting:** The Hub might rate limit if too many requests come in short time from one user (especially unauthenticated or smaller requests). Using an authenticated token helps. Also, if doing many uploads, consider the API call volume. The library endpoints for uploading try to be efficient (upload folder uses multiple files per commit).


-


**Memory use:** If you use` path_or_fileobj` as a file object, huggingface_hub will read it to upload. If it’s large and you already loaded it in memory, that’s heavy. Better to pass file path so it streams from disk. It uses the` requests` library to stream file bytes, which is memory-efficient.


-


**Deleting large files:** Removing a large file from a repo doesn’t immediately free space in the sense of storage quota – because the blob is still in history. For personal use it’s fine, but if you’ve accidentally uploaded something huge and want to reclaim quota, you might need assistance or the upcoming Git LFS prune features (Hugging Face is working on better large file deletion).


-


**Use of**` Repository` **(git) vs API:** If you have to do heavy repository restructuring (like move many files or refactor), sometimes cloning the repo with` Repository(local_dir, clone_from=repo_id)` and using git operations might be easier. That downloads the repo (so network usage to clone) and then you can manipulate and push back. But for typical “upload these files” flows, the direct API methods are easier and avoid needing git installed. Indeed,` upload_file` and co. do not require git on your system; they use HTTP.


-


**Spaces (apps) upload:** The same functions can create and upload to Spaces (just use` repo_type="space"` in create_repo and then upload files including a` app.py` or` README.md` etc). But note that Spaces often have build and runtime, so one might use` upload_folder` to push the whole app directory.


-


**Enterprise or offline mode:** If you’re running in an environment without internet and want to push later, huggingface_hub doesn’t queue changes for later – you’d need connectivity. Possibly use` Repository` to commit locally, then push when online (like a normal git workflow). There’s no built-in offline commit queue.


**Integration examples:**


-


**Integration with training scripts:** You can integrate huggingface_hub uploads in training code. For example, using the Hugging Face Transformers Trainer, there’s an argument` push_to_hub` that behind the scenes uses huggingface_hub to create a repo and upload artifacts at the end of training (even intermediate checkpoints, if configured). Under the hood, it’s doing similar calls to` create_repo` and` upload_folder` . This demonstrates how huggingface_hub can be integrated to automatically publish results.


-


**Integration with CI tools:** Suppose you have a GitHub Actions workflow that, whenever you tag a release of your code, you want to also upload a model. You could use huggingface_hub in a small Python script triggered by CI to log in (using a token stored in secrets) and call` upload_file` or` upload_folder` to push the model. This programmatic approach is much easier than e.g. writing raw curl commands to the Hub APIs.


-


**SpaCy pipelines:** spaCy’s` spacy_huggingface_hub` extension uses huggingface_hub to push spaCy models. If you are a spaCy user, you might simply call` nlp.push_to_hub("model-name")` and behind the curtains, it is calling` create_repo` and` upload_file` for all necessary files (and writing a` README.md` for you). This is an example of huggingface_hub making it easy for other libraries to integrate with the Hub.


-


**Data versioning with Datasets:** If you prepare a dataset and want to share it, you could do` create_repo("my-dataset", repo_type="dataset")` and` upload_folder("data/", repo_id="yourname/my-dataset")` . Now that dataset can be loaded via the` datasets` library by others. Or you can even include a dataset script in the repo for more complex datasets. huggingface_hub’s repo management is what enables you to manage those dataset repos similarly to model repos.


-


**Continuous monitoring:** In production, one might use huggingface_hub to periodically save model checkpoints. For instance, every epoch, save the model locally and call` upload_file` with` revision=f"epoch-{i}"` to push to a branch or tag. This way all training checkpoints are safely stored on the Hub (which could act like an external model store). This integration ensures no checkpoint is lost even if local machine fails, etc.


**Common errors and solutions (uploads):**


-


*Authentication required:* If you get an error like` 401 Unauthorized` or a message about authentication when trying to upload or create a repo, it means you need to provide a token. Solution: use` huggingface-cli login` in terminal (which stores your token in` ~/.huggingface/token` ) **or** pass` token="hf_xxx"` in the function calls. Once logged in, the library picks up the token automatically. If running on a headless environment (like CI), you can set the` HUGGINGFACE_TOKEN` env var or pass token in code.


-


*Repo already exists:* If` create_repo` errors saying repository name already exists, either use a different name or if it’s yours and you want to reuse, set` exist_ok=True` . If it’s not yours (someone else has that name), you must choose a unique name under your namespace (or use your org’s namespace).


-


*File too large:* The Hub currently has a file size limit (around 50GB per file for free users, and an overall repo limit for some tiers). If you attempt to upload something huge (say a 100GB file), you might get an error or failure. In such cases, consider splitting the file if possible (for instance, if it’s a big dataset, split into parts) or contact Hugging Face for support if it’s a legitimate need. Usually, models are under that size. Also, if you hit the total storage quota (for free accounts it’s quite generous, like hundreds of GBs public), you might have to clean up or upgrade plan.


-


*Conflicts:* If someone else (or another process) pushed to the repo at the same time as you, you might encounter a git conflict error when uploading. huggingface_hub might report something like “Tip of branch is behind” or similar. This is rare for personal workflow, but if multiple people collaborate on a repo, coordinate changes or use separate branches then merge. To fix, you might need to pull the latest (if using` Repository` ) or just retry your upload to main if the changes are orthogonal. The hub might auto-merge simple non-conflicting changes (like different files), but two edits to the same file from different sources would conflict. In such a scenario, using` Repository` and doing a manual merge could be necessary.


-


*Large file upload fails in between:* If you have an unstable connection and uploading a multi-GB file fails, the partial upload might be on the Hub’s storage but not committed. You can simply try again; the multi-part upload picks up from scratch for each part that didn’t complete. huggingface_hub may not currently resume partially uploaded parts, so it may start over that part. Ensure you have stable connection or possibly split files. If using AWS or cloud environment, consider uploading from a machine with good connection to avoid frustration.


-


*Naming issues:* Avoid having` path_in_repo` beginning with "/" or "./"; just use relative paths (the library expects that). Also, Windows paths with backslashes should be converted to forward slashes for` path_in_repo` if you construct it; huggingface_hub doesn’t automatically do that. Example: if you do` path_in_repo="models\\weights.bin"` in code, it may treat the "" as escape or just not as intended. Use` "models/weights.bin"` .


-


*Commit history length:* If you do hundreds of commits (like one per epoch for 100 epochs), the repo history will be long but that’s fine. However, if many large files are in history, anyone cloning the repo will get all versions. This is where using git LFS (which huggingface_hub does automatically for large files) helps because old versions reside on remote storage only, not in clone by default. Just be aware that if you want to prune old versions, it’s a manual process (Hub doesn’t purge history by default).


-


*Spaces specific:* If uploading to a Space (repo_type="space"), note that a` README.md` and certain files (like` app.py` or` Gradio interface files` ) are needed for it to run. If after uploading, the Space doesn’t work, double-check you included all needed files. huggingface_hub doesn’t magically create a runtime environment; it just stores files. So test your app locally or follow HF Space guides to ensure correct structure. This is not an error with huggingface_hub per se, but a common pitfall (uploading incomplete app and wondering why it fails).


Using huggingface_hub for repository management empowers you to treat models and datasets with the same DevOps rigor as code – versioning, reviewing changes, and collaboration become easier. Whether you are an individual sharing your first model or an enterprise automating model deployment, these tools cover the workflow. Next, we’ll explore the search capabilities of huggingface_hub, which help you discover content on the Hub programmatically.


### Searching for models and datasets


**What it does and why it’s important:** The Hugging Face Hub hosts an enormous number of models, datasets, and other resources. The **search feature** in huggingface_hub allows you to query this vast collection programmatically to find repositories that meet certain criteria. This is important when you want to discover what's available or filter down to specific types of models without manually browsing the website. For instance, you might want to find all models for text generation in Spanish, or the top downloaded models for image classification, or datasets related to healthcare. Using search via the API enables building dynamic applications (like showing available models in a UI dropdown, etc.) and performing analysis (like retrieving metadata about multiple models). It also helps in automation: a script can find if there’s already a model that matches your need. Instead of hardcoding model IDs, you can search and select models by description, tags, or other properties. In summary, the search functionality turns the Hub into a queryable database of ML assets, which is extremely powerful for both exploration and programmatic selection.


**Syntax and parameters:** The main method for search is` HfApi().search_models()` for models, and similarly` HfApi().search_datasets()` for datasets (and potentially` search_spaces()` for Spaces, though less commonly used).


The` search_models` method returns a list of` ModelInfo` objects that match the query. Key parameters:


-


` query` (str, optional): A free-text query string. It will match against model names, descriptions, tags. E.g., "bert sentiment English" might find relevant models.


-


` filter` (ModelFilter, optional): This is a more structured way to filter by attributes like task, library, language, etc. There’s a class` huggingface_hub.Filter` (or specifically` ModelFilter` ) where you can specify` task` ,` library` ,` language` ,` model_name` , etc. For example,` ModelFilter(task="text-classification", library="pytorch", language="en")` .


-


You can also specify specific tags in the query or filter. The Hub categorizes models with tags like` pipeline-tag:text-classification` ,` language:en` ,` license:apache-2.0` , etc. A filter can incorporate those.


-


` sort` (str, optional): how to sort results. Common sorts include "downloads", "likes", "updated" or "model_id". So if you want top popular models, sort by downloads.


-


` direction` (int, optional): 1 or -1 for ascending/descending. If sort by downloads and direction=-1, that means highest downloads first (descending order).


-


` limit` (int, optional): number of results to return. If not specified, it might return a default (like 10 or 20). If you expect many results and want them all, you can set a high limit (there’s also offset param for paging).


-


` full` (bool, optional): If true, returns more complete info (including the full model card etc.). By default, the results may contain summary info to keep the payload small. Setting full=True may slow down the query because it fetches more data.


-


There's similar structure for` search_datasets` but with a` DatasetInfo` object and slightly different filters (like filtering by dataset task, etc.)


Additionally,` HfApi().list_models()` exists which is like search but without query (just filters or global listing).` list_models` can list by owner, etc.


**Examples of using search:**


1.


*Basic keyword search:*


```text
from huggingface_hub import HfApi  api = HfApi()  results = api.search_models(query="spanish translation", limit=5)  for model in results:      print(model.modelId, ":", model.description)
```


This might find models where "spanish" and "translation" appear in the name or description. For example, it might return something like "Helsinki-NLP/opus-mt-en-es" (an English-to-Spanish translation model) etc. We limited to 5 results for brevity. Each` model` is a ModelInfo with attributes like` modelId` ,` description` ,` downloads` ,` likes` ,` tags` , etc. We print a couple of fields. This simple usage is akin to the search bar on the website.


2.


*Structured filtering:*


Suppose you want all text classification models for sentiment analysis in English. Many such models exist (BERT variants, etc.). You can narrow:


```text
from huggingface_hub import ModelFilter  filt = ModelFilter(task="text-classification", language="en", model_name="sentiment")  results = api.search_models(filter=filt, sort="downloads", direction=-1, limit=10)  for model in results:      print(f"{model.modelId} - Downloads:{model.downloads} - Likes:{model.likes}")
```


Here,` task="text-classification"` will filter models tagged for text classification tasks,` language="en"` ensures English, and` model_name="sentiment"` ensures the model’s name or tags likely involve "sentiment". The filter’s model_name parameter is basically a textual filter on model name, similar to query but narrower. We sort by downloads descending to get the most popular ones first. The output might include models like "distilbert-base-uncased-finetuned-sst-2-english" or "nlptown/bert-base-multilingual-uncased-sentiment". We print some info, showing they are sorted by download count.


3.


*Advanced filtering by library or license:*


Suppose you specifically need a model that can run in TensorFlow.js (so likely a model with a TF.js export). Those are usually tagged with library "transformers" and maybe "tfjs". Or maybe you need only Apache 2.0 licensed models for commercial use. You can do:


```text
filt = ModelFilter(library="keras", license="apache-2.0")  results = api.search_models(filter=filt, sort="updated", direction=-1, limit=5)  for model in results:      print(model.modelId, "-", model.cardData.get('license'))  # cardData might include license too
```


This filter asks for models implemented in Keras (or have Keras weights) and with Apache 2.0 license. We sort by updated (most recently updated first). We then print their license to confirm. This could be useful to find the latest Keras models that are permissively licensed.


Note:` license="apache-2.0"` filter depends on models having that license tag set in their README or metadata. Many do but not all authors tag license properly.


4.


*Using search_datasets:*


Example, find datasets in French related to healthcare:


```text
from huggingface_hub import DatasetFilter  dfilt = DatasetFilter(language="fr", description="medical")  dsets = api.search_datasets(filter=dfilt, sort="downloads", direction=-1, limit=5)  for ds in dsets:      print(ds.id, "-", ds.description[:50])
```


This will filter datasets where language is French and their description likely contains "medical". Sorting by downloads to see popular ones. It prints the dataset ID (like "username/dataset_name") and first part of description.` DatasetFilter` can similarly filter by task or size, etc. Searching datasets is great for discovering relevant data without manual search.


5.


*Full text vs tag search differences:* The free text` query` is broad. If you did` search_models(query="GPT-2", limit=3)` , you’d likely get the GPT-2 model variants. But if you did` ModelFilter(model_name="gpt2")` , that specifically filters model names containing gpt2. Both may yield similar results in this case, but query could also match descriptions (so maybe any model *mentioning* GPT-2). Filters by tags (like` task="text-generation"` ) rely on authors tagging properly. So sometimes combining query with filter is useful: e.g.,` query="GPT-2", filter=ModelFilter(task="text-generation")` to ensure the model is indeed a text generation model.


**Performance considerations:**


-


The search API returns at most 1000 results (server-side limit). If you need more, you have to use offsets (like` api.search_models(..., limit=1000, full=True, offset=1000)` to get next page). But rarely do you need more than top few hundred.


-


Using` full=True` can slow down the response because it fetches complete metadata (like model card content, which can be large). For quick scanning or identification, default (full=False) is faster.


-


Rate limiting: If you call search too frequently (like in a tight loop), you might get rate-limited by the Hub API. It’s generally fine to call occasionally or on demand. If building an app that queries often, consider caching results or implementing a backoff.


-


Sorting by downloads or likes is fine, but sorting by updated or model_id is straightforward. If you do not sort, I believe the default is sort by relevance to query (which is often good enough for queries).


-


Filtering uses server-side indexing: tasks, languages, etc., are indexed, so it’s efficient. Free text query likely uses a search index too. So performance is good, typically sub-second for moderate result sets. Very broad queries (like no query and no filter with a high limit) will naturally take longer, but still manageable if limit is not huge.


-


Keep in mind that the hub content grows daily; results today might differ from results a few months later as new models come.


-


If you plan to use search in a live application, consider the user’s perspective: maybe limiting to a subset (like only your org’s models) might be needed. You can filter by owner or organization using ModelFilter(owner="organization-name") if needed.


-


The` ModelInfo` objects returned contain many fields (like` downloads` ,` tags` ,` pipeline_tag` which is the primary task, etc.). Accessing them is easy (dot notation). If you use` full=True` , you also get` cardData` which is the processed README metadata (like from model card YAML). That might include details like metrics, etc., if the author provided them.


-


Searching Spaces uses` HfApi().list_spaces` or similar. We’ll not focus too deeply on that, but it’s there.


**Integration examples:**


-


**CLI tool integration:** If you were writing a command-line tool that helps users pick a model, you could use search to list models by keyword. For example, a user types a search term and your CLI uses` search_models` to show a numbered list of matching models, then the user selects one to download.


-


**Web app integration:** You could create a web interface that allows filtering models by task or language, by calling` search_models` with those filters when the user changes a dropdown. Hugging Face’s own website does this obviously, but you can make custom dashboards. For instance, a company might filter for all their org’s models and present them in a dashboard with metadata from ModelInfo.


-


**Auto-selection in code:** Suppose your code wants to automatically use the "best" model for a certain task. You could programmatically do something like: search for task text-generation, sort by downloads, get top result’s modelId, and then load that model via Transformers. This way, your code always picks up the currently most popular model for that task (assuming popularity correlates with quality, which might be a simplistic assumption, but it’s a concept).


-


**Metadata analysis:** You can gather interesting stats, e.g., how many models exist for a given language. For instance:


```text
langs = ["en", "fr", "es", "de", "zh"]  for lang in langs:      res = api.search_models(filter=ModelFilter(language=lang), limit=1)      total = res.total  # search_models might set a total count attribute print(f"Models for {lang}: {total}")
```


If` res.total` is available (I think the API returns total count aside from just the returned hits), it tells how many models have that language tag. This could feed into reports or analysis for research on representation of languages in the Hub.


-


**Cross-library scenario:** Perhaps you have a GUI application (like a notebook extension) that uses huggingface_hub to query available models and let users import one. The search API would be used to populate that list dynamically instead of hardcoding known models.


**Common errors and their solutions (search):**


-


*No results found:* If` search_models` returns an empty list and you expected some, double-check filters. Maybe the filter combination is too strict (e.g., language code must match exactly the tag; if a model isn’t tagged properly, it might be missed). Try broadening query or removing one filter to see if results appear. Also check spelling of tags (some languages use full names, some codes, e.g., "english" vs "en").


-


*Using the wrong filter class:* There is` ModelFilter` and` DatasetFilter` (and similar for Spaces). Ensure you use the correct one for` search_models` vs` search_datasets` , otherwise the filter might not apply properly or even error if it has unknown fields.


-


*Invalid sort key:* If you pass a wrong string to` sort` (not one of the allowed fields), the API might ignore it or throw an error. Check documentation or examples for allowed sort values. They’ve been known to support: "model_id", "downloads", "likes", "updated" for models; for datasets possibly "downloads", "updated", etc.


-


*Case sensitivity:* The search query is case-insensitive generally. But filter tags might be case-sensitive (not sure if "language=En" would match or not; likely expects lowercase 'en'). Always use lowercase for languages and tasks (since tag values are usually lower).


-


*Network issues:* If you have no internet or the Hub API is unreachable, search calls will fail. The exception might be something like` requests.ConnectionError` . The solution is obvious (ensure internet, or catch the error and handle gracefully).


-


*Large query result memory:* If you do something like` search_models(limit=1000, full=True)` you might end up with a lot of data in memory (including possibly large model card content for each). This could be heavy if you try to process thousands of models at once. Use filtering to narrow down if possible, or set full=False to only get core info, or do it in batches if needed.


-


*Deprecation note:* In older versions of huggingface_hub, search might have been separate from list or had a different signature (like maybe an argument` search=` in list_models). Using the current stable method as shown above is recommended.


The search feature adds a dynamic edge to using huggingface_hub. It helps in discovering content and building smarter applications that can adapt to what’s available. Now that we’ve covered search, let’s move on to another powerful feature: utilizing the Hub for model inference, which allows running model predictions via huggingface_hub – an advanced but highly useful capability.


### Running inference via the Hub


**What it does and why it’s important:** Hugging Face provides **Inference API** endpoints and hosted inference services that allow you to get predictions from models on the Hub without fully loading them on your local machine. The huggingface_hub library includes tools (like the` InferenceClient` ) that let you send data to these models and get outputs directly. This feature is important for a few reasons:


-


It enables quick prototyping with models you haven’t downloaded – you can test a model’s output with a few API calls, which is great to evaluate models before deciding to download or deploy them.


-


It allows you to leverage powerful models that may be difficult to run locally (due to size or required hardware) by using Hugging Face’s hosted inference (some models are deployed on superclusters accessible via the API, sometimes as a paid service for heavy usage).


-


You can integrate model predictions into applications without needing the model or ML framework on your side – just via HTTP calls. For example, a lightweight backend can call the inference API for text generation from a large model that is too heavy to host itself.


-


The` InferenceClient` in huggingface_hub supports multiple “providers” (like Hugging Face’s own infrastructure or even third-party ones, as mentioned in the doc snippet). It provides a unified interface to call models on different backends, including local ones like OpenAI’s API, which is a new integration (the providers list included` 'openai'` ,` 'replicate'` , etc. – meaning you could call OpenAI’s API through this interface similarly).


In short, running inference through huggingface_hub library is a convenient way to *use* models as a service. It’s particularly handy in production or web apps scenario where you might want to avoid bundling large models. Also, for tasks like automatic model evaluation or piping data through a bunch of models for comparisons, the Inference API can save time and memory.


**Syntax and usage:**


The main class is` huggingface_hub.InferenceClient` . Creating an instance:


```text
from huggingface_hub import InferenceClient  client = InferenceClient(model="gpt2", token=<optional>, provider=<optional>)
```


If you specify` model="gpt2"` , by default it will use Hugging Face’s hosted inference for that model (if available) or load a recommended model for tasks (some tasks might pick a default model if none given, as documentation suggests). The` provider` parameter can specify alternate backends:


-


` 'hf-inference'` (the default, Hugging Face’s own Inference API).


-


` 'replicate'` ,` 'cohere'` ,` 'openai'` , etc., as listed, which if you want to use those providers (you’d need their API keys; huggingface_hub can unify that).


-


` 'auto'` (the default behavior, I think tries HF inference first).


Also` api_key` or` token` param for authentication if required (HF’s free inference is rate-limited but works without token for some models, while others require token if model is gated or you have high volume usage requiring HF Inference API subscription).


Once you have the client, there are task-specific methods, as glimpsed in doc:


Examples:


-


` client.text_generation(prompt, **kwargs)` – generate text from a prompt.


-


` client.text_classification(text)` – classify text (likely returns list of labels and scores).


-


` client.question_answering(question=..., context=...)` – answer a question given context.


-


` client.sentence_similarity(sentence, other_sentences)` – returns similarity scores.


-


` client.translate(text, target_language=...)` – perhaps a method for translation (if not, one can call a model by name with generic method).


-


` client.summarization(text)` – to get summary.


-


` client.conversational` or` chat_completion` for chat models (there were changes: older` ConversationalOutput` removed in favor of` chat_completion` method).


-


` client.audio_to_text(audio_bytes)` – if there’s an ASR model call.


-


` client.image_classification(image=...)` – classify image (it can accept an image path or binary).


-


` client.image_to_text(image=...)` – maybe for captioning.


-


etc. Basically,` InferenceClient` has a method per common pipeline.


Also a generic` client.post(...)` to call any specific pipeline by name or raw API call if needed, but the convenience methods cover most tasks.


Parameters often include:


-


` model` as a param in each call if you want to override the model set in the client (the client can be bound to a model at init, or you can pass a model each time).


-


Additional generation parameters (for text gen: max_length, temperature, etc.), or top_k for QA, threshold for object detection, etc. These align with the HF Inference API parameters.


**Examples:**


1.


*Text generation from a model:*


```text
client = InferenceClient(model="gpt2")  output = client.text_generation("Once upon a time", max_length=30)  print(output)
```


This would produce a continuation of "Once upon a time ...". The output format might be either raw text or a list containing variants (depending on underlying API, possibly a list of generated texts or a single string). Likely something like` \['Once upon a time ...'\]` as return. You can experiment to see exact format. The HF Inference API for text-generation returns a list of generated sequences by default.


2.


*Sentiment analysis via text classification:*


```text
client = InferenceClient(model="distilbert-base-uncased-finetuned-sst-2-english")  result = client.text_classification("I love this library!")  print(result)
```


This should return something like` \[{'label': 'POSITIVE', 'score': 0.9998}\]` . If the model outputs multi-label maybe it returns list of dicts for each label. In this case, SST-2, it’s binary so one label. The` InferenceClient` abstracts the HTTP calls to the model’s pipeline. If you don’t specify a model in` InferenceClient` , calling` text_classification` might pick a default model (likely a generic one HF chooses like "distilbert-base-uncased-finetuned-sst-2" anyway for sentiment). But specifying ensures which model is used.


3.


*Question answering:*


```text
context = "Hugging Face Inc. is a company based in New York City. Its headquarters are in DUMBO, therefore very close to the Manhattan Bridge."  question = "Where is Hugging Face based?"  ans = client.question_answering(question=question, context=context)  print(ans)
```


Should output something like` {'answer': 'New York City', 'score': 0.98, 'start': ..., 'end': ...}` . This uses a default QA model (like distilbert-base-cased-distilled-squad) or one you set in model param. The presence of` 'answer'` indicates what text segment answered the question.


4.


*Object detection on an image:*


```text
client = InferenceClient(model="facebook/detr-resnet-50")  with open("image.jpg", "rb") as f:      img_bytes = f.read()  detections = client.object_detection(image=img_bytes)  for det in detections:      print(det['label'], det['score'], det['box'])
```


This will output detected objects labels, their confidence, and bounding box coordinates. For example: "cat 0.998 {'xmin':..., 'ymin':..., 'xmax':..., 'ymax':...}". The` InferenceClient` identified an object detection model by name and applied it. If the model requires certain format (like DETR returns boxes in normalized coordinates), the API might unify or just pass raw output. Typically HF Inference API normalizes them 0-1 or actual pixel values? Based on doc snippet, it returns list of` ObjectDetectionOutputElement` with bounding box coords and label/score.


5.


*Speech recognition (audio to text):*


```text
client = InferenceClient(model="facebook/wav2vec2-base-960h")  with open("speech.wav", "rb") as f:      audio_bytes = f.read()  text = client.audio_to_text(audio_bytes)  print("Transcription:", text)
```


If the model is a speech recognition model, you get a transcription string out. Possibly simply` 'Hello world'` for example if that’s what was said. The method might internally send the raw wave to HF’s API and get JSON with text or directly text.


6.


*Using other providers:* For example, to use an OpenAI model via the same interface:


```text
client = InferenceClient(provider="openai", api_key="sk-...")  # using your OpenAI API key  completion = client.chat_completion(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello, who are you?"}])  print(completion)
```


This integration means huggingface_hub becomes a one-stop shop to call either HF models or other API providers. The returned format tries to mirror OpenAI’s response maybe. It’s fairly new, as indicated by support for chat completion.


Similarly,` provider="together"` or others require relevant keys.


Note: using external providers isn't exactly running inference *on the Hugging Face Hub* , but the` InferenceClient` has been expanded to cover them for convenience. It's up to user to ensure they have credentials.


**Performance and limits:**


-


**Latency:** Using the Inference API introduces network latency. If you have the model locally, local inference might be faster for repeated calls, but if not, the Inference API saves loading time but each call goes to a server and back. For small payload tasks (like short text classification), latency is typically a few hundred milliseconds to a second. For bigger tasks or larger models (like text generation of long outputs from a big model), it could be several seconds. There's overhead from HTTP plus queueing on server if many requests are being processed.


-


**Rate limiting:** The HF Inference API has rate limits. Unauthenticated or free-tier usage is limited (maybe a certain number of calls per day and concurrent calls). For heavy usage, HF offers paid plans or you self-host. If you exceed limits, you might get errors or slower responses. The` token` parameter can allow a higher rate (if tied to a subscription or to identify you).


-


**Model availability:** Not all models have an inference pipeline on HF's end. Many popular ones do, but if you try some niche model, the Inference API might not support that architecture or might default to returning an error. The client tries to find a pipeline by model’s` pipeline_tag` . If a model is not deployable (like it’s huge or requires custom code), the HF Inference API might not run it. Check the model's page: if it has an "Inference API" widget, then HF’s servers can run it. If not, you might need to run it locally or on Spaces. The client will raise an error if the endpoint returns error.


-


**Output parsing:** The` InferenceClient` methods try to parse JSON responses into Python objects or dictionaries. If something fails, you might get raw text or an exception. Always consider wrapping calls in try/except to handle issues (like model loading error, input too long causing error, etc).


-


**Throughput:** If you want to use the Inference API in parallel (like batch 1000 texts), there are better ways like using the` accelerate` endpoints or the batch support (some pipeline endpoints allow batch input). The client’s high-level methods typically handle one item or a list (some accept list for batch). For example,` text_classification` might accept a list of sentences and return list of predictions, to do it in one API call. Using that is more efficient than a loop of single calls.


-


**Security:** When sending data to a third-party (the Hub servers or other provider), be mindful of data sensitivity. If data is sensitive and you cannot send it off machine, then you wouldn't use this. The library doesn’t send anything anywhere except to the API endpoints you direct (so be sure not to accidentally use openai provider with sensitive data if that’s a concern).


-


**Token gating:** For restricted models (like some large language models require accepting terms or even require being on an access list), the Inference API requires an auth token that has access. If you attempt without token or with a token not authorized, you'll get a` GatedRepoError` or 403. The solution is to log in with a token that has access (and of course ensure you have the rights to use it).


-


**Stability:** The huggingface_hub library's inference support is evolving. Ensure you have a recent version for the best support (v0.26+ for new inference client features, as indicated by the conversation removal etc.). If using older version, maybe only a basic` HfApi().inference` method was there.


**Integration examples:**


-


**User interface integration:** Suppose you have a web app where users input a sentence and want sentiment. Instead of loading a model in your backend (which could be heavy), your backend could simply call` client.text_classification` and return the result. This offloads model serving to HF’s infrastructure. Many small startups do this to avoid maintaining ML servers at first (with caution on speed and cost).


-


**Data pipelines:** If you want to label a dataset automatically using a Hub model, you can write a script that iterates over your data, and for each item, calls the inference API for a particular model. For instance, auto-tagging text with a classification model. This is simpler with huggingface_hub’s client than writing your own HTTP calls.


-


**Chatbots or assistants:** Using the` InferenceClient.chat_completion` with an HF or OpenAI model, you can integrate it into an application’s logic. The unified interface means you could swap out` provider="openai"` to` provider="hf-inference"` with a different model as needed, without changing too much code, which is nice for comparative experiments or fallback mechanisms.


-


**Testing multiple models:** If evaluating model quality, you could use the inference client to get predictions from different models easily by changing` client = InferenceClient(model="modelA")` vs modelB. No need to download them. This is useful in a benchmark or analysis setting.


-


**Edge cases handling:** If building robust systems, you might use inference with fallback logic: e.g., try a large model via HF API, if it fails or times out, catch exception and maybe try a smaller model or alternative path. huggingface_hub’s exceptions like` InferenceTimeoutError` can be caught to implement such logic.


**Common errors and solutions (inference):**


-


*InferenceTimeoutError:* If the model doesn’t respond in time or is loading too slowly, you might get a timeout. The doc suggests it raises` InferenceTimeoutError` for 503 or timeouts. Solution: either handle it by retrying or using a simpler model or check if your input is too large (maybe reduce input length).


-


*HTTP errors (401, 403):* As mentioned, authentication issues for private/gated models – get a token with access and do` client = InferenceClient(token="hf_yourtoken")` .


-


*Model missing pipeline:* If you get an error like "Model XYZ does not have a pipeline for this task", it means HF’s inference doesn’t support it. Possibly because model type is unsupported or task mismatched. Ensure you called the correct method (like calling` text_classification` on a model that's actually a translation model could yield weird output or error). Or maybe the model architecture isn't supported by HF's backend. In that case, you might have to use a different model or run it locally. Also, ensure you're using the right method for what you want: e.g., use` client.summarization` for summarization models rather than` text_generation` .


-


*Invalid input format:* If you pass an image incorrectly (like passing file path string instead of bytes, or an unsupported format), you might get an error or gibberish output. Always open and read binary for images/audio. For audio, HF expects wav or supported audio bytes. If your audio is in a weird format, convert to wav or use their accepted formats.


-


*Large input causing OOM:* If you send a very long text to a model that can’t handle it (like thousands of tokens to BERT), the server might error (maybe Inference API would return a message about sequence too long or just a generic error). The huggingface_hub might not explicitly propagate that except as HTTP error. Solution: chunk the input or use a model with longer context (like some longformer).


-


*Providing incompatible args:* If you use a method with a param not applicable to that model (e.g.,` client.text_generation(..., some_unsupported_param=True)` you might get an error or it might ignore it. Check HF Inference API docs for supported parameters per task. For text generation, it’s okay to pass top_k, top_p, etc. For text classification, there's typically no extra param beyond maybe` multi_label` . The huggingface_hub doc likely outlines which` **kwargs` apply.


-


*Legacy usage:* The huggingface_hub library earlier had an` HfApi().inference(model, inputs)` method (and older even had` hf_hub_request` etc.). Those may still exist but` InferenceClient` is the current recommended. If reading older code or StackOverflow answers, adapt them to use` InferenceClient` .


-


*OpenAI provider usage issues:* If using` provider="openai"` but you didn't install openai package or similar, the huggingface_hub might handle it internally (the doc suggests they integrated it directly). But ensure you pass correct model names and message format as per OpenAI’s spec, otherwise you'll get an error directly from OpenAI’s API forwarded.


-


*Thread safety:* If you plan to use one` InferenceClient` from multiple threads, I'm not certain if it's fully thread-safe (likely it is as it doesn't hold much state except maybe session). But to be safe, one client per thread or use locks might be considered.


-


*Cost:* Not an error per se, but remember that if you use the Inference API at scale, it might incur costs or usage limits. For instance, OpenAI API calls will charge your key. HF’s free Inference has limited capacity (for heavy use you’d need the paid Inference API or you run your own).


-


*Deprecated InferenceApi class:* Huggingface_hub older versions had an` InferenceApi` class with a different usage. If you come across that, note that` InferenceClient` is the newer approach. The older one required passing model and pipeline, etc. The new one is more user-friendly with dedicated methods.


Running inference via huggingface_hub opens up a streamlined way to utilize models across various scenarios. It ties together much of what we discussed: you can search for a model, pick one, and then directly use it through this interface. With that, we've explored advanced usage; next, we will discuss how to maximize performance and best practices for writing robust code with huggingface_hub.


## Advanced usage and optimization


### Performance optimization


In certain scenarios – such as deploying in production or handling large volumes of data – you’ll want to optimize how you use **huggingface_hub** to ensure efficiency in terms of speed and memory. While huggingface_hub is primarily an I/O-focused library (dealing with network and file operations), there are still several strategies to improve performance:


**1. Efficient caching and memory management:** By default, huggingface_hub caches downloaded files on disk under` ~/.cache/huggingface/hub` . This prevents re-downloading the same data multiple times, which is a major speed gain for iterative experiments or repeated script runs. To optimize memory usage, note that huggingface_hub streams downloads to disk rather than loading entire files into memory – this is good for large files, as it avoids needing RAM proportional to file size. For example, when you call` hf_hub_download` on a 5GB model weight, it will write it in chunks to your cache directory, so memory usage stays low. One thing to manage manually is **clearing the cache** if needed: over time, your cache may accumulate old versions of models. If disk space is a concern or you want to free up space after experiments, you can delete the` ~/.cache/huggingface/hub` directory or specific subfolders within it. The library also offers a utility` huggingface_hub.scan_cache_dir()` and` huggingface_hub.delete_cache()` (or via CLI` huggingface-cli delete-cache` ) to help manage this. Clearing unnecessary cache files can indirectly improve performance by freeing disk I/O for relevant files and ensuring you don’t run out of disk (which would drastically slow everything or cause failures). Another tip: if working on multiple machines or CI systems, you can set the` HF_HOME` environment variable to point to a faster or bigger disk location for caching. For instance, pointing it to an SSD path can speed up read/write of model files compared to a network-mounted drive.


**2. Speed optimization with concurrency:** Huggingface_hub’s` snapshot_download` uses multiple threads (` max_workers` ) to download repository files in parallel. If you are downloading a repository with many files (like a dataset with thousands of small files or a model with dozens of weight shards), using concurrency can significantly reduce total download time by parallelizing the I/O. The default` max_workers=8` is a balanced choice for most cases, but you can adjust it. If you know your network and disk can handle more parallelism, you could increase` max_workers` to 16 or 32 when calling` snapshot_download` for even faster retrieval (keeping in mind diminishing returns and potential server limits). Conversely, if you are in a limited environment (like low bandwidth or a very slow disk), you might reduce concurrency to avoid overhead. The key is that huggingface_hub already attempts to maximize bandwidth usage by concurrent downloads of different files. For example, if you are downloading the entire Stable Diffusion model repository which has multiple large files, all will be fetched in parallel. This can nearly saturate your network line and use all available throughput, thus optimizing speed.


**3. Partial downloads and filtering:** If you don't need every file from a repository, avoid downloading unnecessary data. The` snapshot_download` function offers` allow_patterns` and` ignore_patterns` to include/exclude specific files by name or glob pattern. Using these can drastically cut down download time and storage. For instance, if a repo contains model weights for multiple frameworks (PyTorch, TensorFlow, ONNX, etc.) and you only need the PyTorch weights, you could do:


```text
snapshot_download("model/repo", allow_patterns="*.bin")
```


This will fetch only files ending in` .bin` , skipping others (like` .h5` or` .onnx` files). Similarly, if a dataset repo has raw data plus processed data, and you only need the processed data, pattern match accordingly. By transferring less data, you speed up the process. This is both a performance optimization and a bandwidth saver.


**4. Avoid redundant operations and leverage local checks:** The huggingface_hub library is smart about not re-downloading files that are already present and up-to-date. It uses file hashes (ETags) to detect changes. To optimize, make sure to let it do its work: avoid manually deleting cache or forcing downloads unless necessary. If you call` hf_hub_download` on the same file multiple times in one run, know that it will hit the cache after the first time – it’s not going to download again. Also,` snapshot_download` will skip files that are already fully in cache matching the right revision. This means you can call` snapshot_download` repeatedly (even in parallel processes) and only the first call will do actual network transfers while subsequent ones find files locally. This design encourages idempotent usage. One caution: if you use the` local_dir` option to output to a custom folder, the caching mechanism is slightly different (it will still create a` .cache` folder in that local_dir for metadata). But in general, trust the built-in caching to reduce redundant I/O.


**5. Use background threads or asynchronous calls for large operations:** In some advanced cases (like building an app or GUI), you might not want a` snapshot_download` to block your main thread for potentially minutes. Huggingface_hub functions are synchronous (there is not currently an async API, though` run_as_future=True` parameter exists for some upload functions). One trick is to manage large downloads in a separate thread or process, so that your main program can continue doing other tasks (like updating a progress bar or allowing user interaction). This doesn’t make the download itself faster, but it can improve the responsiveness of an application using huggingface_hub. For example, in a PyQt GUI, you could start a QThread that runs` snapshot_download` , and meanwhile show progress or messages in the UI thread. Another scenario: if you need to download multiple separate models, you could do them concurrently using Python’s` concurrent.futures.ThreadPoolExecutor` or` ProcessPoolExecutor` – just ensure not to saturate your disk or network beyond its capacity. Given that huggingface_hub and` requests` are I/O-bound, using threads is suitable (the GIL will release during network I/O). For instance, to download three different model repos in parallel:


```text
import concurrent.futures  model_ids = ["model1", "model2", "model3"]  with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:      futures = [executor.submit(snapshot_download, mid) for mid in model_ids]      for future in concurrent.futures.as_completed(futures):          print(f"Downloaded: {future.result()}")
```


This way the three download operations run in parallel (each will internally spawn more threads for their files as well, which might be a bit heavy but generally okay if you have the bandwidth). The outcome is a more efficient wall-clock time usage, though CPU might spike due to many threads. Monitor your system’s ability to handle it; you can dial down` max_workers` either at ThreadPool or snapshot level accordingly.


**6. Profiling and benchmarking:** If performance is critical, consider profiling where your bottlenecks are. For huggingface_hub usage, you might find that network throughput or disk write speed is the limiting factor, not the Python overhead. However, overhead can come from repeated hashing of files or processing of the repository structure. For very large models composed of many files (like 50k small files), listing them and performing checks can itself take time. Huggingface_hub tries to optimize by doing parallel downloads, but each file’s integrity check (hashing) or decompressing (if compression is involved) uses CPU. Using tools like` cProfile` or even timing prints can help identify if a particular step is slow. For example, scanning a giant cache directory can be slow; if you frequently do that, maybe reduce frequency or focus on relevant parts. If using huggingface_hub in a training loop (less common, but maybe pushing intermediate results each epoch), ensure those calls are not unnecessarily slowing training – maybe accumulate changes and push less often, or push asynchronously so training can continue.


**7. Utilizing chunk-level deduplication for large files (Xet integration):** As discussed in earlier sections, huggingface_hub introduced integration with Xet storage which deduplicates large file content at the chunk level. This primarily benefits the upload/download of updated large files when only small parts change (like model checkpoints after a few training epochs). If you are in a scenario where you repeatedly push and pull large models that only slightly change, leveraging this is beneficial. Ensuring you have huggingface_hub >=0.32 and that your repo is Xet-enabled (new repos are by default as of mid-2025[huggingface.co](https://huggingface.co/docs/hub/en/storage-backends#:~:text=To%20start%20using%20Xet%20Storage,,are%20the%20default%20%2085) ) means that subsequent downloads might be faster because unchanged parts of large files can be fetched from cache or a CDN edge. On upload, it means you send only the diff. While this is mostly automatic, a performance note: if you run into issues or need even more speed, consider environment variables` HF_XET_HIGH_PERFORMANCE=1` (if supported) to let the hf_xet use higher resource usage for faster transfers[huggingface.co](https://huggingface.co/docs/hub/en/storage-backends#:~:text=,level%20deduplication) . This uses more parallelism in chunk upload/download within a file. Also, enabling symlinks on Windows (as earlier) ties into this – Xet uses symlinks in the cache to avoid duplicating chunks, so having symlink support gives optimal dedup storage usage[dev.to](https://dev.to/danc/huggingface-fix-you-either-need-to-activate-developer-mode-or-to-run-python-as-an-administrator-307o#:~:text=C:/Users/xxx/AppData/Local/Programs/Python/Python39/lib/site,cache#limitations) .


In summary, the performance of huggingface_hub operations can be optimized by smart caching, parallelism, and limiting work to what’s necessary. Most critical is to avoid redundant network transfers (rely on caching and partial download features) and to utilize concurrency where appropriate to utilize available bandwidth and CPU across multiple tasks. By applying these strategies, you can significantly reduce the time and resources required to manage models and datasets, especially at scale.


### Best practices


Writing code that interacts with external resources like the Hugging Face Hub requires careful handling of errors, versioning, and organization. Here are some best practices to follow when using the **huggingface_hub** library:


**1. Organize code for clarity and maintainability:** It’s good practice to encapsulate Hub interactions in separate functions or modules in your codebase. For example, if you have a training script that at the end pushes a model to the Hub, isolate the pushing logic into a function` push_to_hub(model_dir, repo_id)` rather than scattering` create_repo` and` upload_file` calls throughout your script. This makes it easier to maintain or modify the process (like adding commit messages or error handling in one place). Use descriptive commit messages when uploading files. For instance:


```text
commit_msg = f"Upload epoch {epoch} model weights"  upload_file(..., commit_message=commit_msg)
```


Clear commit messages (and descriptions, via` commit_description` if needed) are helpful if others (or you later) browse the repo’s history on the Hub. Also, prefer consistent repository naming and structure. A typical convention: name your model repo similar to your project and include important details (like` username/project-modelName` ). Within repos, follow standard file names (like` README.md` for descriptions,` config.json` for model config, etc.) so tools and users can easily understand the content. Hugging Face Hub reads README to display model cards, so include usage examples, metrics, license info in that – it’s not code per se, but “documentation as code” in your repo. You can even add metadata at the top of README in YAML which gets parsed (tags, license, etc.). This helps in search and clarity (we saw how Skops integration adds secure model card info for scikit-learn models for example).


**2. Robust error handling and fallbacks:** When dealing with network operations, things can go wrong. Wrap huggingface_hub calls in try/except blocks where appropriate and handle exceptions gracefully. For example, if calling` hf_hub_download` , catch` requests.HTTPError` or` RepositoryNotFoundError` to handle cases like missing repo or connection issues. Provide the user (or log) with informative messages:


```text
try:      hf_hub_download(repo_id="user/repo", filename="weights.bin")  except RepositoryNotFoundError:      print("The repository does not exist or you don't have access. Please check the name or permissions.")      return except requests.HTTPError as http_err:      print(f"HTTP error occurred: {http_err}")      # Possibly retry or abort depending on status code.
```


Similarly for upload operations, catch` HfHubHTTPError` (generic error) or more specific errors like` GatedRepoError` for gated models (if pushing to a gated model? more likely for downloading gated content) and handle them. In long-running applications or scripts, consider adding retry logic for transient failures. For instance, if a download fails due to a network hiccup, you could wait a few seconds and try once more. The huggingface_hub does some built-in retries for certain operations, but you can add your own if needed (especially around calls to the Inference API, where you might get timeouts or rate limit errors). The key is not to let an unhandled exception crash your entire program if it can be anticipated.


Another best practice is to validate inputs early. If your code expects a user to input a model name or path, and you then feed that to huggingface_hub, do some basic checks: e.g., ensure string is not empty, maybe verify format (like contains a "/" if expecting "user/model"). This way, you catch easy mistakes before making an API call that would error.


**3. Testing and environment consistency:** Ensure that the versions of huggingface_hub and related libraries are compatible with your code. For example, if you rely on features introduced in huggingface_hub 0.13 (like` snapshot_download` ) or 0.26 (like the new InferenceClient), make sure to specify a minimum version in your` requirements.txt` or setup. This prevents issues where someone runs your code with an older version that lacks those methods. Similarly, if you're using Transformers or Datasets library in conjunction, keep them updated to versions known to work well with huggingface_hub version (the libraries are decoupled but they often bump huggingface_hub dependency as needed). As a practice, pin or at least range-limit your dependency versions in deployment scenarios to avoid surprises from future breaking changes.


When it comes to *testing* , if your project involves pushing to or pulling from the Hub, consider writing integration tests that might run in a controlled environment (possibly with a test account or a temporary repo) to ensure those flows work. For example, a test could create a small repo, upload a tiny text file, then download it and compare content. Hugging Face Hub even provides a staging environment (HF Hub has a concept of "staging" domain for internal testing) – not commonly used by external devs, but for critical applications you could use a separate account or org as a “test sandbox”. Always remember to not hardcode sensitive tokens in tests; use environment variables or a config. If writing open-source code that uses huggingface_hub, encourage users to set up` huggingface-cli login` or provide tokens through environment variables (like` HUGGINGFACEHUB_API_TOKEN` ) rather than embedding them.


**4. Documentation and communicative code:** This may sound general, but it's very pertinent: document your usage of huggingface_hub in your project’s README or code comments. Many users may not be familiar with huggingface_hub usage, so explain what certain steps do. For instance:


```text
# Download the pre-trained model weights from Hugging Face Hub (caches for reuse).  snapshot_download("bert-base-uncased", revision="v1.0")
```


A comment like that clarifies your intent. If you rely on environment variables (like` HF_HOME` or` HUGGINGFACE_TOKEN` ), mention those in your docs so others know how to set up their environment. In a collaborative setting or for future you, these notes are invaluable. If distributing code, abide by the responsibilities like informing users if large downloads will happen or if data is being uploaded externally.


**5. Security and privacy considerations:** Be mindful of what you upload to the Hub. Do not accidentally upload sensitive data or credentials. A common mistake would be pushing an entire working directory which might include config files with secrets. Use` ignore_patterns` in` upload_folder` to exclude such files, or better, separate sensitive info out of the directory you plan to upload. The Hub is public by default (unless you create a private repo), so assume anything you push could be seen by others. This is a best practice from a dev workflow perspective too: treat model artifacts and code separately from private data. Also, verify the license of models/datasets you use – huggingface_hub can filter by license; ensure if you integrate a model, its license is compatible with your use (the Hub model cards often specify license, and huggingface_hub's ModelInfo provides it via tags or card data).


**6. Use of CLI and CI tools:** Hugging Face provides a CLI (` huggingface-cli` ) that can do many tasks like` huggingface-cli login` (to store token),` huggingface-cli repo create` and` huggingface-cli upload` . In some cases, using CLI in a script (or better, in a CI pipeline step) might simplify things. For instance, after training, you could call a subprocess to run` huggingface-cli upload ./model-dir user/model-name --exclude *.tmp` to push the entire model. Underneath it uses huggingface_hub, but the CLI could handle some complexity. However, the Python API is more flexible in-code. On CI, consider caching the huggingface cache between runs (if using GitHub Actions, you can cache` ~/.cache/huggingface` ). This way, subsequent runs don't redownload the same base models – speeding up CI and reducing network usage.


**7. Community engagement:** If your code uses huggingface_hub to publish models, encourage collaboration by providing a good model card (README) and possibly enabling the Community Pipeline on the Hub (there's a "Community" tab where people can open discussion threads or issues on your model repo). As a best practice, monitor that if relevant (subscribe to notifications for your model repos), so if someone finds a problem, you can address it. Also, keep your published models updated if you fix issues – you can push new commits and use tags or branches to mark versions. huggingface_hub makes it easy to manage versions via` revision` parameter, so leverage that: tag stable releases of your model with version numbers (and maybe push a tag commit via` HfApi().create_tag` if the API supports, or via git if using` Repository` ). This allows users to pin their downloads to a specific version for reproducibility.


Following these best practices ensures that your use of the huggingface_hub library is not only effective but also maintainable, secure, and user-friendly. By organizing your code, handling potential issues, and documenting your approach, you make integration with the Hugging Face Hub a smooth experience for both yourself and others who interact with your code or models.


## Real-world applications


To illustrate how the **huggingface_hub** library is used in practice, let's look at several detailed case studies and examples from industry and open-source projects:


**Case study 1: collaborative development of a large language model (LLM)** – *BigScience BLOOM*
*Industry/Application:* Open scientific collaboration for a massive multilingual language model.
**Description:** The BigScience BLOOM project was a year-long initiative involving over a thousand researchers to create a 176-billion-parameter multilingual model. They used Hugging Face Hub extensively to share intermediate checkpoints, evaluation results, and eventually the final model. Huggingface_hub was crucial for managing these artifacts. Throughout development, they regularly pushed model checkpoints (which are huge, split into many files due to size) to a private Hub repository accessible to project collaborators. This allowed different teams (working on training, evaluation, tokenization, etc.) to fetch the latest checkpoints without manual file transfer. The Hub’s versioning also let them tag milestones (like after certain training phases). Performance metrics and analysis notebooks were also uploaded to the repo or shared via the Hub (for transparency). When BLOOM was finally released, they made the repository public on the Hub, enabling anyone to download it or use it via the Inference API. The huggingface_hub library enabled automated integration in their training pipeline: at set intervals, the training script (running on a supercomputer) likely used huggingface_hub’s` HfApi` to upload the newest model shard files to the Hub. In case of training interruption, this meant progress was saved off-site. The result is one of the largest models publicly available, hosted on the Hub with dozens of files and an informative model card. This case shows huggingface_hub’s ability to handle very large files (through chunked uploads and git-LFS integration) and facilitate massive collaboration, where people from around the world can fetch updates or contribute via PRs to the repo’s README and code.


**Case study 2: deploying models in a production service** – *Text classification in a startup’s API*
*Industry/Application:* SaaS providing content moderation for social media platforms.
**Description:** Suppose a startup builds a content moderation service that uses machine learning to detect toxic or inappropriate content. Rather than training a model from scratch, they leverage a pre-trained transformer model fine-tuned for toxicity detection (such as Jigsaw's Detoxify model). Their engineering team uses huggingface_hub to pull this model into their system. In their CI/CD pipeline, they have a step like:


```text
python -c "from huggingface_hub import snapshot_download; snapshot_download('unitary/toxic-bert')"
```


This downloads the model to the build environment, where they then package it into a Docker image for their API service. By pinning a specific version (perhaps a git tag or commit hash of the model repo), they ensure consistency across deployments. In production, they run the model on incoming text. Over time, as new better models appear (or they fine-tune their own on more data), they store those on the Hub as well (maybe under their own account). During a deployment, they can switch the model used by changing the repo name or revision in an environment variable. Because huggingface_hub makes fetching models simple and reliable, they don’t have to manually manage model files or worry about where to store them – the Hub is the single source of truth. Additionally, for quick inference or if they want to avoid hosting the model themselves, at low traffic times they might even use the Inference API. For example, if their volume is small, their backend could call` InferenceClient` to classify content via the Hub, saving them from maintaining a GPU server. As they scale, they can bring it in-house by just downloading the model and deploying on their own infra. This flexibility (cloud versus local) is enabled by huggingface_hub’s consistent interface in both scenarios. The real-world benefit is faster iteration and deployment: the team can upgrade models by updating a config pointing to a new Hub repo, rather than dealing with binary files and storage.


**Case study 3: open-source library integration** – *spaCy 3’s Hugging Face Hub integration*
*Industry/Application:* NLP library enhancement for model sharing.
**Description:** spaCy is a popular NLP library. In version 3, they enabled training and sharing spaCy pipelines on Hugging Face Hub. They even created a command` spacy push` that uses huggingface_hub under the hood (via the` spacy-huggingface-hub` plugin). When a spaCy user trains a new model for, say, named entity recognition, they can run` spacy push <repo_name>` and the library will package the pipeline and use huggingface_hub to create a repo (if needed) and upload all relevant files (model weights, vocabulary, configuration, etc.). Conversely,` spacy pull <repo_name>` will fetch a pipeline from the Hub and load it into spaCy. This real-world integration greatly lowers the barrier to sharing models in the NLP community. The Hub becomes an extension of spaCy’s model storage. Underneath, huggingface_hub’s` create_repo` and` upload_folder` are used to send the pipeline artifacts. The plugin also auto-generates a README for the Hub repo, including metrics and usage examples. For instance, after training a French NER model,` spacy push` might call:


```text
create_repo(name, organization=org, exist_ok=True)  upload_folder(folder_path="./training_artifact", repo_id=f"{org}/{name}", commit_message="Push spaCy pipeline")
```


This case demonstrates how huggingface_hub can be embedded in other tools to provide a seamless user experience. spaCy’s users benefit from easy model distribution, and Hugging Face Hub benefits from more diverse models being available. The open-source nature means users can inspect the plugin code (which indeed uses huggingface_hub’s API behind the scenes). This integration has seen actual usage with hundreds of spaCy models on the Hub now. It validates huggingface_hub’s design: an external library can depend on it without heavy overhead, relying on its stability and features like authentication and error handling.


**Case study 4: multi-modal AI demo deployments** – *Gradio Spaces with huggingface_hub*
*Industry/Application:* AI demo web apps (visualization, public interaction).
**Description:** Hugging Face Spaces is a platform for hosting ML demos (often with Gradio or Streamlit). When developers create a Space, they essentially create a Git repo under their account’s Space namespace. They push code (like` app.py` ) and sometimes model weights to that repo. huggingface_hub is how these files get there. For instance, an individual builds an image captioning demo: they use huggingface_hub to download a pre-trained image captioning model (like BLIP) in their Space’s container at runtime, or more efficiently, they reference it via the Hub. Actually, a better example: they want to include the model weights in the Space so that it doesn't download each time. They could commit the model files to the Space repo. To do that, they might use huggingface_hub on their local machine:


```text
huggingface-cli repo create my-space --type space  huggingface-cli upload ./blip-model-files --repo my-username/my-space
```


This uploads model files to the Space repository via huggingface_hub (the CLI uses the library internally). Now, when the Space runs, the model is already present and loads quickly. In the Space’s code, they might still use huggingface_hub to ensure they have the latest or to pull ancillary files. Many Spaces actually just call pipeline =` transformers.pipeline("image-captioning", model="blip-model")` which under the hood calls huggingface_hub to fetch the model. The Spaces environment is tightly integrated with Hub repositories – any update via huggingface_hub triggers the Space to rebuild. A real-world example is the Stable Diffusion demo Space: the Space repo likely doesn’t contain the full 4GB model (for legal reasons they download at runtime after user accepts a license). They leverage huggingface_hub in the Space code to` snapshot_download("CompVis/stable-diffusion-v1-4")` on first run. The caching ensures subsequent runs on the same hardware skip re-download. This demonstrates huggingface_hub’s role in enabling not just model sharing, but live applications that fetch models on-demand, which is crucial in a dynamic demo service.


**Case study 5: private model sharing within an organization** – *Internal MLOps platform*
*Industry/Application:* Enterprise collaboration on proprietary models.
**Description:** Consider a large company with multiple data science teams. They set up a Hugging Face Hub organization (private) to host their models. Using huggingface_hub, they build an internal tool that lists available models and their descriptions, so team members can easily find what’s been done. This tool uses` HfApi().search_models(filter=ModelFilter(author="our-org"))` to get all models under their organization, then displays them with their` modelId` ,` pipeline_tag` (task), and maybe` downloads` as a proxy for usage. When someone wants to use a model, the tool can either provide a direct` curl` link or just call` hf_hub_download` on the user’s behalf to deliver the weights. Also, for compliance, they tag each model with a license or internal approval tag via the model card metadata. huggingface_hub is used in their CI to automatically push new models when a project’s training job finishes. The integration might look like: after training, the pipeline triggers a Python script that uses` huggingface_hub.HfApi().create_repo` (if not exists) and` upload_folder` to push the model and evaluation results. Perhaps they also upload metrics as part of the model card (the training script might even open the` README.md` from Hub (via` huggingface_hub.HfApi().download_repo` ) and append the latest metrics, then push a commit using` Repository` class or direct API for editing files). In production, their model-serving infrastructure might similarly pull from the Hub. The advantage here is centralization – all teams use the Hub as the source, and huggingface_hub provides the programmatic interface to integrate that into their custom tools. This is a real scenario many companies adopt, as Hugging Face Hub can be a private model registry. huggingface_hub’s design – with fine-grained permission tokens, repo privacy settings, and organization scopes – supports that. For example, they might use a read-only token in production services to fetch models without risking any modifications, and a write token in CI for publishing. Using those tokens with huggingface_hub is straightforward (pass` token=...` to functions or set` HUGGINGFACE_HUB_TOKEN` env var on the CI runner). The outcome is a smoother MLOps workflow: less emailing of model files or confusion about versions – everything is versioned on the Hub and accessible via the Python API.


These case studies highlight the versatility of huggingface_hub across research, product development, open-source, and enterprise contexts. Whether it’s powering massive collaborations like BLOOM, simplifying deployment for startups, integrating into libraries like spaCy, enabling interactive demos on Spaces, or streamlining internal model sharing, huggingface_hub has proven its value in real-world applications. Each scenario leverages slightly different features – but all benefit from the core capabilities: easy push/pull of models and datasets, search and discovery, and reliable hosting via the Hugging Face Hub platform.


## Alternatives and comparisons


### Detailed comparison table


When considering the huggingface_hub library, it's useful to compare it with other tools and libraries that provide similar functionality for model management and distribution. Below is a comparison table of **huggingface_hub** with three alternative approaches: **TensorFlow Hub** (for TensorFlow models), and **PyTorch Hub** (the model loading mechanism within PyTorch). This comparison covers various aspects:


Aspect **huggingface_hub (Hugging Face)** **TensorFlow Hub** (tensorflow_hub) **PyTorch Hub** (torch.hub)


**Features** - Stores models, datasets, spaces (apps) in a unified Hub.
- Versioning via git commits and tags.
- Integrated web UI for model cards & inference widgets.
- API/CLI for upload & download of files.
- Supports multiple frameworks (PyTorch, TF, JAX, etc.).
- Provide Inference API for hosted inferencing. - Repository of pre-trained TF models (SavedModels).
-` tensorflow_hub` library for loading models as Keras layers or SavedModel.
- Focus on TensorFlow-specific features (like reuse as KerasLayer).
- Some versioning, but typically models are static URLs (with one version). - Allows loading models (or parts of models) from GitHub repos via` torch.hub.load.`
- Features some official model collections (e.g., vision models).
- Basic functionality: fetches a script and weights from a repo and imports a model definition.


**Performance** - Download speeds enhanced by CloudFront CDN.
-` snapshot_download` allows concurrent file downloads (multi-threaded).
- Uses file hashing to avoid re-downloads of unchanged files.
- Caching of files locally for reuse across sessions.
- Large file support via chunking (git-lfs or hf-xet for deduplication). - Models are typically downloaded as one` .tar` or similar – uses TensorFlow's caching (e.g., under` ~/.keras` ).
- Performance is good for single-file models via HTTP, but no explicit multi-thread download for a single model (most TF models are single-file or a couple files).
- No CDN – served via Google storage or publisher's site, performance varies. - Downloads from GitHub (single thread). Possibly slower for large weights (and reliant on GitHub's bandwidth).
- Caches the downloaded scripts/weights (in` ~/.torch/hub` ).
- Loading is usually one file at a time (no parallel part downloads).


**Learning curve** - Gentle learning curve: simple pip install and high-level functions (` hf_hub_download` , etc.).
- Clear documentation and many examples (Hub docs, Transformers integration guides).
- Concepts: need to grasp Hub model naming (“namespace/model”) and huggingface-cli login for private repos. - For TF users, very straightforward:` tensorflow_hub.load(url)` returns a model. Minimal extra concepts if you know TF.
- Models identified by URLs, which you might need to find on tfhub.dev website (less integrated search in-code). - If using torch.hub for official models: one-line load for known models is easy (e.g.,` torch.hub.load('pytorch/vision', 'resnet50')` ).
- For community models, need to know the GitHub repo and function name – a bit harder (need to read that repo’s instructions).
- Some potential friction if dependencies are missing (model code might require other packages).


**Community support** - Large, active community on Hugging Face forums and GitHub.
- Many community-contributed models (750K+ public models.
- Official support from Hugging Face team, frequent updates.
- Integration with many libraries (Transformers, Diffusers, spaCy, etc.) means lots of usage examples. - TensorFlow Hub has a decent community among TF users. Models curated by Google/partners mostly.
- Smaller selection compared to HF (especially for newest research models, which often go to HF first nowadays).
- Google provides support through TF docs and forum; not as community-driven in contributions (many models are official or research-set). - PyTorch Hub’s community usage is moderate. Some well-known models (esp. vision) are on it; others prefer HF Hub or direct GitHub readme instructions.
- Support is mainly through PyTorch documentation and GitHub issues for specific repos.
- PyTorch team doesn't curate community models, so quality/support varies per repo.


**Documentation quality** - Hugging Face provides extensive docs for huggingface_hub (API references, how-to guides).
- Many tutorial blogs, videos available due to popularity.
- Each model on Hub often has a README with usage which huggingface_hub can fetch or is displayed on website. - TensorFlow Hub has a website (tfhub.dev) with model pages and usage snippets, plus the TensorFlow docs site explains using` tensorflow_hub` library.
- Documentation is good for official models; community models vary in documentation quality. - PyTorch Hub usage is documented in PyTorch docs (small section). Model repos on GitHub might have readmes, but not standardized.
- Overall documentation is weaker, and users might need to dig into source to see available functions/weights.


**License differences** - huggingface_hub library itself is Apache 2.0 (permissive).
- Models on HF Hub carry their own licenses (displayed on model page and accessible via metadata). HF encourages specifying license and allows filtering by it.
- The platform imposes no additional license on the content, just the content's license and Hub terms of service. - TensorFlow Hub content typically uses licenses specified by publishers (often Apache 2 or Creative Commons for research models). The tfhub.dev site might indicate license, but filtering by license is not prominent.
- The library` tensorflow_hub` is Apache 2.0 as part of TF ecosystem. - PyTorch Hub is part of PyTorch (BSD-style license for PyTorch code).
- Models loaded via torch.hub come from GitHub repos, which have their own licenses (could be MIT, Apache, etc.). It's up to the user to check those repos for license files. No centralized license listing.


**When to use each** - **huggingface_hub** is ideal when you want a one-stop solution for hosting and retrieving models in a framework-agnostic way. Great for sharing with community or across teams; offers rich ecosystem (tracking, discussions, inference API). Use if you value ease of use, broad support, and cloud features. Even if using PyTorch or TF, HF Hub can manage those models (via Transformers integration or direct load).
- Especially useful if you need to handle many models or large files, or want built-in versioning and collaboration. - **TensorFlow Hub** is suitable if your work is strictly in TensorFlow/Keras and you prefer to use Google's ecosystem. It's straightforward for loading TF SavedModels as layers.
- Use it if you specifically need models that are only on tfhub.dev or want to publish models primarily for TF users. However, for new projects, many TF users still host on HF Hub due to broader reach. - **PyTorch Hub** is a quick way to load PyTorch models from GitHub, especially official ones (vision, etc.). It's a fit if there's a well-known repo (like` pytorch/vision` ) and you want minimal external dependencies.
- Use if you already have models on GitHub with loading code and don't require the extra features of HF Hub. It's somewhat limited to PyTorch code and doesn't manage large files as smoothly (beyond GitHub's normal).


*(Table Legend: Hugging Face refers to the huggingface_hub Python library and the Hugging Face Hub platform; TensorFlow Hub refers to the*` tensorflow_hub` *library and tfhub.dev; PyTorch Hub refers to torch.hub functionality in PyTorch; Manual Git+LFS refers to using git repositories and Git Large File Storage to store model files without specialized hub libraries.)*


### Migration guide


Migrating models and workflows from one system to another can be a delicate process. Here we focus on migrating **to** the huggingface_hub library and Hub platform, as well as considerations if moving away from it, though the latter is less common given its widespread adoption. This guide will help you transition from alternatives (like TensorFlow Hub or custom storage) to huggingface_hub, and also outline what to consider if migrating content off huggingface_hub.


**When to migrate to huggingface_hub:**


If you find that your current model management solution is limiting collaboration, visibility, or ease of use, it’s a good time to migrate. For example, you might have a bunch of weight files on an S3 bucket or Google Drive that you share via links – moving these to Hugging Face Hub will immediately provide version control, nice web presentation (model card with instructions), and easier usage through the API. Or perhaps you have models on TensorFlow Hub but you want to reach a broader community (including PyTorch users); hosting on HF Hub makes your model framework-agnostic (you can provide both .bin and .pb in one place and let users pick). Another common scenario is consolidating numerous model files from Git LFS or PyTorch Hub into a single coherent repository on HF Hub where they can be searched and annotated.


**Step-by-step migration process (to huggingface_hub):**


1.


**Set up an account and repository:** Create a Hugging Face account (or organization for group projects). Use the web UI or` huggingface-cli` to create a new repo for your model. You can do:


```text
huggingface-cli login   # if not done already, to get credentials  huggingface-cli repo create my-model
```


This will create` username/my-model` on the Hub. You can also specify` --type dataset` or` --type space` if relevant, but for models the default type is fine.


2.


**Prepare your files:** Gather the model files you want to migrate. This might include:


-


Model weights (e.g.,` .bin` for PyTorch,` .h5` or SavedModel folder for TF, etc.).


-


Model configuration (if any, like a JSON defining architecture or vocab).


-


Tokenizer files (vocabulary, merges, special tokens – if you have them separately).


-


Any code necessary for using the model (though generally, if it’s standard architecture, users will use their library’s classes).


-


A README.md file as a model card. In migration, if migrating from tfhub or others, you likely have some documentation; convert it to markdown. Include description, intended use, examples, and license.


-


(Optional) Add a model card metadata section at top (YAML between` ---` markers) to specify things like` license` ,` language` ,` tags` , which help discovery.


3.


**Upload files to the Hub:** Use huggingface_hub to push these files. You can script it in Python:


```text
from huggingface_hub import upload_file, create_repo  repo_id = "username/my-model"  create_repo(repo_id, exist_ok=True)  # ensure repo exists  files = ["pytorch_model.bin", "config.json", "tokenizer.json", "README.md"]  for file in files:      upload_file(path_or_fileobj=file, path_in_repo=file, repo_id=repo_id, commit_message="Initial commit")
```


Or use the CLI` huggingface-cli upload ./my_model_files/* --repo username/my-model` . If your model is large (several GBs), consider splitting the uploads (the library will anyway do it in chunks internally). After this step, the files are on the Hub.


4.


**Set model metadata (if not already):** On the Hub website, check your model. You can add some tags via the UI (like languages or tasks) if you didn’t in README YAML. Ensure the license is set correctly (very important if migrating an open model from elsewhere – e.g., TensorFlow Hub models often have Apache 2 or CC licenses, reflect that on HF Hub for clarity).


5.


**Update usage references:** In code or documentation where you referred to the old location, update it to use huggingface_hub. For example:


-


If you had` tensorflow_hub.load("https://tfhub.dev/author/model/1")` , you might replace this with` tf.keras.models.load_model(hf_hub_download("username/my-model", "model.h5"))` , or even better, if you also convert the model to a PyTorch or a Transformers pipeline, show usage in those frameworks. But minimally, instruct users how to fetch from the new HF repo: e.g.,` from huggingface_hub import hf_hub_download` .


-


If migrating from PyTorch Hub, where usage was` torch.hub.load('author/repo', 'model_fn')` , now you might direct them to either use Transformers or directly load state dict via HF. Possibly provide a small code snippet in the model card for both PyTorch and TF if applicable.


-


For your own pipelines or CI, change any retrieval scripts to use` snapshot_download("username/my-model")` instead of custom logic.


6.


**Common pitfalls to avoid during migration:**


-


*Missing files:* Ensure you didn't forget to upload something like a vocabulary or a special config (common if models come with multiple files). If the model fails to load from Hub, double-check file presence and names.


-


*Large file handling:* If any file >50MB, it will be handled by git LFS on HF Hub. Usually transparent, but ensure you're using a recent huggingface_hub (v0.13+) which handles LFS pointers automatically. If users are downloading via git clone without LFS, they might get pointer files only. It's better to guide them to use huggingface_hub or` git lfs install` etc.


-


*Retaining version history:* If you care about old versions from tfhub or your own, you could create tags for them on HF Hub. For instance, if tfhub had` model/1` and` model/2` as versions, create tags v1 and v2 on HF (you might need to upload v1 checkpoint too if you want to preserve it). huggingface_hub’s` create_tag` via API or UI can be used after pushing the state corresponding to that version.


-


*Communication:* Let your users or team know about the migration. Possibly keep the old source available for a grace period but mark it as deprecated with a pointer to the HF Hub. For example, if migrating from tfhub, you might add a notice on tfhub model page: "This model is now maintained on Hugging Face Hub at ...".


**Migrating from huggingface_hub to something else:** (not common, but consider scenarios like moving off public hub to internal storage, or if a company goes from HF Hub to their own solution due to policy)


-


If using HF Hub Enterprise on your own servers, the huggingface_hub API can switch endpoint via` endpoint` parameter, so code hardly changes – you'd mostly just use a different URL and token.


-


If fully migrating off, you'd need to rewrite or replace` hf_hub_download` calls with something else. Perhaps you switch to storing models on AWS S3. In that case, the migration would be: upload files to S3, and replace code that does` hf_hub_download(repo, file)` with code using boto3 to get from S3. You lose the integrated versioning, so you'd have to specify paths or versions manually (could use S3 object versioning or separate bucket folders).


-


If moving to TensorFlow Hub or PyTorch Hub: you'd convert model format if needed and follow their publication process (which might be more manual or involve contacting Google for tfhub). You'd then update code to that format.


-


If leaving HF due to licensing or other reason, ensure you delete or make private the model on HF if needed (and inform users it's moved). Use` HfApi().delete_repo` if deprecating.


**Common pitfalls in migrating away:**


-


Loss of community features (no model cards, no built-in inference) – you'll need to cover those gaps if important.


-


Breaking backward compatibility: if others rely on` from_pretrained("you/your-model")` , their code breaks when you remove it. Consider keeping a stub model on HF with perhaps a small README explaining the move, and maybe a dummy weight file or redirect link (though HF doesn't support automatic redirect to external).


-


Data and model size limits: ensure your new platform can handle large files and bandwidth that HF did well.


In conclusion, migrating to huggingface_hub usually brings a net positive in functionality and is relatively straightforward: it often involves uploading files and tweaking references. Migrating away is more complex and usually only done if absolutely required, given the rich feature set and community on the Hub. Planning migrations with these steps and considerations will help minimize disruptions and preserve the usability of your models across platforms.


## Resources and further reading


### Official resources


-


**Hugging Face Hub Documentation:** The official docs for the Hub Python library are very comprehensive. You can find them here: Hugging Face Hub Python Library Docs (latest version) – this includes guides on how to use` huggingface_hub` , how-to examples (uploading a model, downloading, using the Inference API), and reference for every function and class. It’s the first place to look for up-to-date usage information[huggingface.co](https://huggingface.co/docs/huggingface_hub/en/index#:~:text=Hub%20client%20library)[huggingface.co](https://huggingface.co/docs/huggingface_hub/en/index#:~:text=Hub%20client%20library) .


-


**GitHub Repository (source code):** The huggingface_hub library’s source is on GitHub: huggingface/huggingface_hub – GitHub. This is useful if you want to see the implementation or contribute. It also hosts the issue tracker where you can report bugs or see planned enhancements.


-


**Hugging Face Hub Web UI:** The platform itself at hf.co – through the web interface you can explore models, datasets, and spaces. Each model page often has an “Use in library” snippet that shows example code for huggingface_hub or Transformers to load that model. For example, the BERT base model page shows how to load it with Transformers and via curl requests[huggingface.co](https://huggingface.co/blog/keras-hub-integration#:~:text=The%20Hugging%20Face%20Hub%20is,first%20batch%20of%2033%20models) .


-


**PyPI page for huggingface_hub:** huggingface-hub on PyPI – contains installation instructions and release history. You can check the release history to know the current version and changes. For instance, as of writing, version 0.14.1 is available[huggingface.co](https://huggingface.co/docs/huggingface_hub/en/index#:~:text=main%20v0,EN%20FR%20HI%20KO%20TM) (check PyPI for newer versions).


-


**Official Hugging Face Tutorials:** Hugging Face provides tutorials (often via their blog or Colab notebooks) – for example, “How to upload a model to Hugging Face Hub” and “Using Hugging Face Hub in your training pipeline”. These can be found on the Hugging Face website’s “Learn” section or blog. They are official in the sense they’re written by Hugging Face team or experts and often appear on the Hugging Face Medium blog or documentation site.


### Community resources


-


**Hugging Face Forums (Discussion Hub):** The official forum at discuss.huggingface.co is very active. There are categories for “Hub” and “huggingface_hub” where users ask questions about using the Hub, troubleshooting issues, etc. For example, if you encounter a specific error, searching the forum often yields someone who had a similar issue and got help. The Hugging Face team and community members (like Transformers wizards) often answer there.


-


**Stack Overflow (Tags: huggingface-hub, huggingface-transformers):** Stack Overflow has many Q&A on Hugging Face usage. You can follow the huggingface-hub tag for questions specifically about the Hub API. Also, the huggingface-transformers tag often includes relevant discussions, since many questions about loading models overlap with hub usage. Common solved problems like “ModuleNotFoundError for huggingface_hub” or “how to push model programmatically” are covered there[stackoverflow.com](https://stackoverflow.com/questions/79298879/modulenotfounderror-no-module-named-huggingface-hub-inference-types#:~:text=Root%20Cause%20and%20Fix)[stackoverflow.com](https://stackoverflow.com/questions/73960201/modulenotfounderror-no-module-named-huggingface-hub-utils-using-anaconda/74127613#:~:text=I%20had%20the%20same%20problem,resolved%20the%20issue%20for%20me) .


-


**Reddit communities:** Subreddits like r/MachineLearning or r/LanguageTechnology sometimes have threads about Hugging Face releases or how-tos. There’s also a lesser-known r/huggingface subreddit dedicated to HF (small but occasionally active). These can provide more free-form discussion or user experiences.


-


**Hugging Face Discord:** Hugging Face has an official Discord server where the community and some HF staff chat. There are channels for “#beginners”, “#huggingface-hub”, etc. You can ask quick questions there and often get responses in real-time. It’s also a good place for news on new features (like when huggingface_hub gets updated). The invite link can usually be found on the Hugging Face website footer or forums.


-


**GitHub Discussions:** On the huggingface_hub GitHub repository, there might be a Discussions tab. If enabled, it’s a place to ask for help or discuss features (less formal than issues). Many Hugging Face libraries have this (Transformers does; huggingface_hub’s repo may also have Q&A in issues or discussions).


-


**Developer talks and podcasts:** Hugging Face team members often appear on podcasts or YouTube interviews (e.g., the “Gradient Dissent” podcast by W&B had episodes with HF folks). While not strictly support, these give deeper insight into why and how to use the Hub effectively and the philosophy behind it. Also, the “Hugging Face Hugs” community events (talks) often cover tips on using the Hub.


### Learning materials


-


**Hugging Face Course (free online course):** Hugging Face offers a free course on 🤗 Hugging Face (on their website huggingface.co/learn or via GitHub). While it focuses on Transformers, it includes chapters on the Hub (like how to share models and datasets). It guides you through practical exercises of using huggingface_hub (e.g., at the end of the course you upload a model to the Hub).


-


**Books:**


-


*Natural Language Processing with Transformers* by Lewis et al. (O’Reilly, 2021) – co-written by HF’s Thomas Wolf. It covers using Hugging Face ecosystem. In it, there are sections on the Hub and how to fine-tune and share models. It’s a great resource for those doing NLP.


-


*Transformers for Natural Language Processing* by Denis Rothman – also touches on HF and likely the Hub in context of model management.


-


*Machine Learning Engineering* by Andriy Burkov – not specific to HF, but if you’re reading it, consider how huggingface_hub can be part of an ML engineering pipeline (the book emphasizes model versioning and collaboration, which HF Hub addresses).


-


**Online courses and tutorials:**


-


Coursera has a course in the DeepLearning.AI NLP specialization which now includes Hugging Face lessons. Specifically, the Course on Hugging Face co-produced by DeepLearning.AI covers using Hub models etc.


-


YouTube channels like “HuggingFace” (official) and “AssemblyAI” or “The AI Epiphany” often post tutorial videos. For example, “How to Share Your Models with Hugging Face” or “Using Hugging Face Inference API in Python” – these step-by-step videos can be very instructive if you prefer visual learning.


-


**Interactive tutorials/notebooks:**


-


Google Colab notebooks: many blog posts or forum solutions link to Colabs showing how to push a model to Hub or how to load from Hub. Searching for “huggingface hub colab” yields some ready-to-run notebooks.


-


Hugging Face’s own notebook repository (on GitHub or HF hub under` huggingface/notebooks` ) – they have a notebook for hub demonstrating uploading a model, etc.


-


**Code repositories with examples:**


-


The huggingface_hub GitHub repo’s examples directory if it exists. If not, the Transformers repository has examples and uses huggingface_hub under the hood (so browsing Transformers code, e.g., the` Trainer.push_to_hub()` implementation, can teach best practices).


-


Other open-source projects that integrated huggingface_hub (like spaCy’s plugin, or adapters like adapter-transformers) – reading their usage can provide real-world patterns.


-


**Blogs and articles:**


-


Hugging Face Blog on medium (or hf.co/blog): They have posts like “How to deploy your model to Hugging Face Hub”[huggingface.co](https://huggingface.co/docs/huggingface_hub/en/concepts/git_vs_http#:~:text=HfApi:%20a%20flexible%20and%20convenient,HTTP%20client) and case studies from companies using the Hub.


-


Towards Data Science has community articles with titles like “Sharing Models with Hugging Face Hub” or “5 Tips for Hugging Face Hub You Might Not Know” – these can be insightful for intermediate tips.


-


Company engineering blogs (e.g., Twitter’s blog or Grammarly’s blog) occasionally mention using HF Hub in their pipeline, which can offer perspective on large-scale usage.


In summary, there is a wealth of resources to learn about huggingface_hub – from official docs and courses to community Q&A and third-party tutorials. The ecosystem is growing, and staying updated is fairly easy given the active community. Always make sure to check the date of resources (the Hub evolves, e.g., new commands or InferenceClient updates), and cross-reference with the official documentation for the latest usage patterns.


## FAQs about huggingface_hub library in Python


Below is a curated list of **frequently asked questions (FAQs)** regarding the huggingface_hub library and its usage in Python, organized by category. Each answer is concise (2-3 sentences) to address the question clearly.


### 1. Installation and setup


1.


**Q:** How do I install the huggingface_hub library in Python?
**A:** You can install it via pip:` pip install huggingface_hub` . This will download the latest version from PyPI and install its dependencies.


2.


**Q:** Do I need a specific Python version for huggingface_hub?
**A:** Huggingface_hub requires Python 3.7 or above. It’s compatible with Python 3.8, 3.9, 3.10, etc., so ensure you have a recent Python version.


3.


**Q:** I installed transformers; do I need to install huggingface_hub separately?
**A:** Recent versions of transformers include huggingface_hub as a dependency and install it automatically. You typically don't need to install it separately if you have an up-to-date transformers library.


4.


**Q:** How do I upgrade huggingface_hub to the latest version?
**A:** Use pip:` pip install -U huggingface_hub` . This fetches and installs the newest release so you have the latest features and bug fixes.


5.


**Q:** I get a` ModuleNotFoundError: No module named 'huggingface_hub'` even after installation – what do I do?
**A:** Ensure the installation succeeded and that you’re using the same Python environment. You might need to pip install again in the correct environment or check for typos in the import (use` import huggingface_hub` exactly).


6.


**Q:** Can I use huggingface_hub in a Jupyter notebook or Google Colab?
**A:** Yes, huggingface_hub works in notebooks and Colab. Just` !pip install huggingface_hub` at the top of the notebook if it's not already installed, then import and use it normally.


7.


**Q:** Do I need to create a Hugging Face account to use huggingface_hub?
**A:** You can use some features (like downloading public models) without an account. But to push content or access private models, you need an account and to log in with your token.


8.


**Q:** How do I log in to Hugging Face Hub from Python?
**A:** You can authenticate by running` huggingface-cli login` in a terminal and entering your token (or using` HfApi().login(token)` ). Once logged in, the library uses your saved token automatically for authorized requests.


9.


**Q:** Where does huggingface_hub store my login token or credentials?
**A:** The CLI stores your token in the file` ~/.huggingface/token` . The huggingface_hub library reads from there so you don't have to pass the token manually each time.


10.


**Q:** I’m behind a proxy/firewall – how can I use huggingface_hub?
**A:** You can configure environment variables like` HTTP_PROXY` and` HTTPS_PROXY` for your proxy. huggingface_hub uses` requests` , so it respects these proxy settings for network calls.


11.


**Q:** Is huggingface_hub available for Conda installation?
**A:** While not on the main conda channels, you can still use pip within a conda environment to install huggingface_hub. Alternatively, check conda-forge; huggingface_hub might be available there.


12.


**Q:** I get SSL certificate errors when using huggingface_hub – how to fix?
**A:** This usually means local certificate issues. Updating the` certifi` package (` pip install -U certifi` ) often resolves it. You can also set` verify=False` in requests (not recommended security-wise) or ensure your system trusts the root CAs.


13.


**Q:** Does huggingface_hub work on Windows/Mac/Linux?
**A:** Yes, it’s a pure Python library and works across operating systems. Ensure Git LFS is installed for some operations on Windows if you plan to push large files.


14.


**Q:** Do I need Git installed for huggingface_hub to work?
**A:** You don’t need git for most high-level operations (the library uses HTTP APIs). If you use the` Repository` class to clone and push via git or working with git-based approach, then you should have git installed.


15.


**Q:** How can I verify that huggingface_hub is installed and find its version?
**A:** In Python, do` import huggingface_hub; print(huggingface_hub.__version__)` . This will print the current version of the library you have, confirming it's installed.


16.


**Q:** The installation fails with a` No such file or directory: 'README.md'` error – what should I do?
**A:** This occasionally happens with pip if the package metadata had an issue. Try upgrading pip (` pip install -U pip` ) and reinstall. Usually the huggingface_hub package installs smoothly on latest pip.


17.


**Q:** Can I install huggingface_hub from source (GitHub)?
**A:** Yes. You can clone the GitHub repo and run` pip install .` inside it, or directly` pip install git+https://github.com/huggingface/huggingface_hub` . This installs the bleeding-edge version (useful if you need a fix before the next release).


18.


**Q:** Is huggingface_hub part of the` transformers` library or separate?
**A:** It’s a separate library. Transformers uses it under the hood but huggingface_hub can be used independently in any project. It focuses just on Hub interactions.


19.


**Q:** After installing, I got an error` AttributeError: 'str' object has no attribute 'decode'` on import – what's wrong?
**A:** This might indicate an older version of one of the dependencies. Try upgrading` packaging` or` pyyaml` . Ensure huggingface_hub is up-to-date. This error is not typical; a clean environment reinstall often fixes it.


20.


**Q:** How do I uninstall huggingface_hub if needed?
**A:** Simply run` pip uninstall huggingface_hub` . This will remove the library from your environment. If you also want to remove cached files, you can delete the` ~/.cache/huggingface` directory manually.


21.


**Q:** The library is installed, but` huggingface-cli` command is not found – how do I get it?
**A:** The CLI tool is provided by` huggingface_hub` . If` huggingface-cli` isn't in your PATH, make sure the Python scripts directory (where pip installs executables) is in your PATH. On reinstallation it should create that entry point.


22.


**Q:** Do I need Git LFS for downloading models via huggingface_hub?
**A:** Not for downloading – huggingface_hub handles large files (stored via LFS on the backend) through HTTP automatically. You only need Git LFS if you intend to push large files using the git CLI or` Repository` class.


23.


**Q:** I'm on an offline environment; can I use huggingface_hub?
**A:** Without internet, huggingface_hub cannot reach the Hub of course. But you could manually download models elsewhere and transfer them. huggingface_hub itself doesn't provide offline functionality beyond reading from cache if you already downloaded something. Ensure to pre-fetch needed models in an environment that has connectivity.


24.


**Q:** Is huggingface_hub available in other languages or only Python?
**A:** The main huggingface_hub client is Python. There are third-party clients in other languages (like` huggingface_hub` for JavaScript or R), but the official is Python. For other languages, one often uses direct HTTP calls or those community SDKs.


25.


**Q:** After login, how do I use the token in code explicitly?
**A:** You can pass` token=<your_token>` to functions like` HfApi()` methods or` upload_file` . Alternatively, set the` HUGGINGFACEHUB_API_TOKEN` or` HF_API_TOKEN` environment variable in your script so the library picks it up automatically.


26.


**Q:** Can I use huggingface_hub in corporate settings behind authenticating proxy?
**A:** Yes, but you need to configure your environment to use the proxy (username/password for proxy might go in the` HTTP_PROXY` environment var as` http://user:pass@proxy:port` ). The` requests` library used by huggingface_hub will respect those.


27.


**Q:** The installation on Apple Silicon Mac is slow/stuck at building wheels – what can I do?
**A:** huggingface_hub doesn't have heavy dependencies, but ensure that build tools are in place (Xcode command line tools on Mac). You can also try installing via conda if pip is problematic. Typically though,` pip install huggingface_hub` should be quick as it's pure Python.


28.


**Q:** I'm encountering` ImportError: cannot import name 'hf_hub_download'` – why?
**A:** This likely means you have an older version of huggingface_hub where` hf_hub_download` didn't exist or you misspelled something. Update to the latest version. In versions below 0.1X, that function might not exist (the library matured quickly around version 0.5+).


29.


**Q:** Does huggingface_hub work with GPUs or does it require any specific hardware?
**A:** Huggingface_hub itself is just about model management; it doesn't interact with GPUs directly or require them. It's pure Python and network I/O, so it works regardless of CPU/GPU presence.


30.


**Q:** My organization uses an internal HF Hub instance (Enterprise). How do I configure the library to use that?
**A:** You can set the endpoint by doing` HfApi(endpoint="https://your.hub.url")` and similarly for` snapshot_download` or others with` library_utils.configure_hf_hub_endpoint` . Provide the URL and an appropriate token (from that instance). This directs huggingface_hub to talk to your internal server instead of huggingface.co.


### 2. Basic usage and syntax


1.


**Q:** How do I download a file from the Hugging Face Hub in Python?
**A:** Use the` hf_hub_download(repo_id, filename, ...)` function from` huggingface_hub` . For example:` hf_hub_download("bert-base-uncased", "pytorch_model.bin")` will fetch the BERT model weights file and return the local file path.


2.


**Q:** What does` repo_id` mean in huggingface_hub functions?
**A:**` repo_id` is the model or dataset identifier on the Hub, typically in the format` "username/repo_name"` . For official models, it might just be` "bert-base-uncased"` (a shortcut for` huggingface/bert-base-uncased` ). Always include the namespace (user or org) for clarity.


3.


**Q:** How can I get the list of all files in a Hub repository?
**A:** You can use` HfApi().list_repo_files(repo_id="user/repo")` . This returns a list of filenames in that repo (at the default revision, usually main).


4.


**Q:** What's the difference between` hf_hub_download` and` snapshot_download` ?
**A:**` hf_hub_download` downloads a single specified file from a repo.` snapshot_download` downloads a whole repository (all files or a subset) at a particular revision, essentially giving you a local snapshot of the repo contents.


5.


**Q:** How do I specify a particular version or commit of a model to download?
**A:** Use the` revision` parameter. You can set it to a branch name, tag, or specific commit hash. For example:` hf_hub_download("user/model", "weights.bin", revision="v1.0")` will get the file from the v1.0 tag.


6.


**Q:** What happens if the file I try to download with hf_hub_download is already cached?
**A:** The function will detect it (via etag) and not re-download. It will return the path to the cached file immediately, so you don't waste bandwidth or time.


7.


**Q:** How do I download an entire model repository programmatically?
**A:** Use` snapshot_download("username/model-name")` . This will fetch every file in the repo (you can filter with patterns if needed) and store them in your cache or specified directory, mirroring the repo structure.


8.


**Q:** Can I get metadata like description or likes of a model via the API?
**A:** Yes, using` HfApi().model_info("user/model")` returns a ModelInfo object with attributes like` description` ,` downloads` ,` likes` ,` tags` , etc. Similarly` HfApi().dataset_info` for datasets.


9.


**Q:** How do I use huggingface_hub to load a model directly into Transformers?
**A:** The Transformers library already uses huggingface_hub under the hood. You can just do` AutoModel.from_pretrained("user/model")` . If you wanted to manually use huggingface_hub, you'd download the files and then load them e.g.,` weights_path = hf_hub_download(...); model = AutoModel.from_pretrained(weights_path)` .


10.


**Q:** How can I search for models on the Hub using Python?
**A:** Use` HfApi().search_models(query="keyword", filter=ModelFilter(...))` . For example:` HfApi().search_models(query="translation", filter=ModelFilter(language="fr"))` to find French translation models.


11.


**Q:** What is the Repository class and when would I use it?
**A:**` Repository` (from` huggingface_hub` ) is a convenience for doing git operations on a Hub repo. It allows you to clone a repo locally, commit changes, and push back. You'd use it if you prefer treating the repo like a git repo in code (for instance, to add multiple files and one commit).


12.


**Q:** How do I create a new repository on the Hugging Face Hub via Python?
**A:** Use` HfApi().create_repo(name="repo-name", token=<token>, organization=<orgName>, repo_type="model")` . Or equivalently` create_repo("org/repo-name")` from high-level functions. This will make an empty repo you can then upload to.


13.


**Q:** How can I upload a single file to my Hub repo in code?
**A:** Use` upload_file` . For example:` upload_file(path_or_fileobj="model.bin", path_in_repo="model.bin", repo_id="username/my-model", commit_message="add model weight")` . Ensure you've logged in or pass a token.


14.


**Q:** If I want to upload a whole folder with many files, what's the easiest way?
**A:** Use` upload_folder(folder_path="my_model_dir", repo_id="username/my-model", commit_message="initial model commit")` . It will iterate through and upload all files in that folder to the repository.


15.


**Q:** What happens if I call upload_file on a file that already exists in the repo?
**A:** It will overwrite that file by uploading the new content and create a new commit. The previous version remains in the repo history, but the latest revision will have your updated file.


16.


**Q:** How do I add a README or model card to my repository with huggingface_hub?
**A:** A README is just a file named "README.md" in the repo. You can create it locally (markdown format), then use` upload_file(..., path_in_repo="README.md", ...)` to push it. On the Hub, it will render as the model card.


17.


**Q:** Is there a way to delete a file from the Hub repo programmatically?
**A:** Yes,` HfApi().delete_file(path_in_repo="file.xyz", repo_id="user/model", revision="main")` will delete that file in a commit. Alternatively,` Repository` class usage or the web UI can delete, but direct API deletion is possible as shown.


18.


**Q:** How can I delete or remove an entire repository using huggingface_hub?
**A:** You can call` HfApi().delete_repo(repo_id="user/repo-name")` . Be cautious, as this is irreversible via API (though the Hub might require confirmation via website for safety). Usually, you might just make it private or rename if needed, but deletion is possible.


19.


**Q:** What is an HF token and when do I need to supply it manually?
**A:** An HF token is your access key (found on your HF account settings) for the Hub. If you are performing actions that need authentication (like uploading, or downloading a private model), you need to supply it. If you logged in via CLI, the library picks it up automatically; otherwise, you can pass it via the` token` parameter in HfApi or functions like` create_repo` .


20.


**Q:** How do I check how many downloads or likes my model has from code?
**A:** Use` model_info = HfApi().model_info("user/model")` . Then` model_info.downloads` gives the download count and` model_info.likes` the number of likes. These are updated periodically on the Hub.


21.


**Q:** Can I use huggingface_hub to list all models in my account or organization?
**A:** Yes.` HfApi().list_models(author="your-username")` will list models under your account. Or` HfApi().list_models(author="organization-name")` for an org. This returns a list of ModelInfo for each.


22.


**Q:** How do I specify which branch of the repo to upload to or download from?
**A:** Use the` revision` parameter. For downloads, set` revision="branch-name"` ; for uploads, likewise, e.g.,` upload_file(..., repo_id="user/repo@dev")` or use` HfApi().upload_file(..., revision="dev")` . If branch doesn't exist on upload, it will create it.


23.


**Q:** If I have multiple files to download, is there a way to get them all at once?
**A:**` snapshot_download` is what you want to download multiple files (the whole repo) in one call. If you just want specific files, you can either loop with` hf_hub_download` or use` allow_patterns` in snapshot_download to restrict to certain file types/names.


24.


**Q:** What does huggingface_hub do if a download is interrupted?
**A:** Partially downloaded files remain in the cache with a temporary name. On the next call, the library will attempt to resume the download (using HTTP range requests if possible). So often it will pick up where it left off rather than starting over.


25.


**Q:** Where does huggingface_hub save downloaded files by default?
**A:** By default, files are cached in` ~/.cache/huggingface/hub` . Within that, there’s a directory structure by model and commit. If you want to change it, set the` HF_HOME` env var to point to a different cache root.


26.


**Q:** How can I specify a custom cache directory for downloads?
**A:** You can pass` cache_dir` parameter in functions like` hf_hub_download` or` snapshot_download` to direct files to a specific path. Alternatively set environment variable` HF_HOME` or` HF_HUB_CACHE` to your desired cache location.


27.


**Q:** How can I check if a certain model repository exists on the Hub via the API?
**A:** Use` HfApi().model_info("user/model")` and catch exceptions. If it raises` HfHubHTTPError` (404), the repo likely doesn’t exist. There's no dedicated "exists" function, but this approach works. You could also use` list_models(author="user")` and see if it's in the list.


28.


**Q:** Is it possible to fetch the commit history or specific commits via huggingface_hub?
**A:** The API currently doesn't expose commit history directly via Python (other than by knowing revision hashes or tags). You can use the GitHub-like interface: for models, huggingface_hub acts as a git remote. If needed, use` Repository` to clone and then use git commands to inspect history. But for most cases, you'll use tags or specific commit IDs if you kept track of them externally.


29.


**Q:** How do I use huggingface_hub to utilize the Inference API (call model inference on the Hub)?
**A:** Use` InferenceClient` from huggingface_hub. For example:` client = InferenceClient(model="facebook/bart-large-cnn"); result = client.summarization("Long text here...")` . That will call the Hub’s hosted model and return a summary. This uses your token if required for rate limits.


30.


**Q:** Can huggingface_hub manage space (App) repos as well?
**A:** Yes, it can. Just specify` repo_type="space"` in create_repo or endpoints. Uploading files works the same, but you might want to push a full app folder to a space. Everything from huggingface_hub’s perspective is similar, just the namespace differs (` username/space_name` with type space). You can also start spaces via the web UI and clone them with` Repository` class.


### 3. Features and functionality (40 questions)


1.


**Q:** What key features does huggingface_hub provide?
**A:** It provides programmatic access to Hugging Face Hub: downloading models/datasets, uploading and versioning files, searching and listing models, managing repos (create/delete), and even running inference through hosted models[pypi.org](https://pypi.org/project/huggingface-hub/#:~:text=Welcome%20to%20the%20huggingface_hub%20library) . Essentially, it lets you integrate Hub operations into your Python code.


2.


**Q:** Does huggingface_hub handle large files (like 10GB models) automatically?
**A:** Yes, large files are stored via git LFS on the Hub, but huggingface_hub hides that complexity. It downloads them in chunks over HTTP (so it won't load 10GB into memory at once)[huggingface.co](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/inference_client#:~:text=Raises) . It also uses caching to avoid repeated downloads. Uploading large files is handled with chunked uploads behind the scenes (it might break it into parts for reliability).


3.


**Q:** How does caching in huggingface_hub work?
**A:** When you download a file, it's saved in the local cache directory with a name including its sha256 hash. Next time you request the same file (same repo and revision), huggingface_hub sees it's present and up-to-date (via ETag) and returns it from cache. You can manually clear cache if needed, but by default it keeps files for reuse, speeding up subsequent runs.


4.


**Q:** What is an ETag and how does huggingface_hub use it?
**A:** An ETag is a unique identifier (hash) for a specific version of a file on the server. huggingface_hub uses ETags to determine if the cached file matches the latest on the Hub. If they match, it skips downloading; if not, it fetches the new version.


5.


**Q:** Can huggingface_hub resume interrupted downloads?
**A:** Yes, if a download was partially done, the next download attempt will include a range header to continue from where it left off (assuming the server supports it, and the Hub does). This means you usually don't have to start from zero after a break.


6.


**Q:** How do I filter which files to download with snapshot_download?
**A:** Use the` allow_patterns` or` ignore_patterns` parameters. For example:` snapshot_download("user/repo", allow_patterns="*.bin")` will only download files ending in .bin. This is handy if the repo has many files but you only need certain ones.


7.


**Q:** What is the purpose of ModelFilter and DatasetFilter in search?
**A:** These filters let you narrow search results by specific fields like task, language, library, etc. For instance,` ModelFilter(task="text-classification", library="pytorch")` ensures you get models tagged for text classification and built for PyTorch. It's a structured way to filter beyond the full-text query.


8.


**Q:** How does huggingface_hub integrate with the Transformers library?
**A:** Transformers internally uses huggingface_hub for model and config downloads. So when you call` AutoModel.from_pretrained("model_name")` , under the hood it calls huggingface_hub's download functions to fetch weights/config. Also, the` push_to_hub` methods in Transformers use huggingface_hub to upload models to the Hub.


9.


**Q:** Can I use huggingface_hub to push model training logs or metrics?
**A:** Yes, you can upload any file. Some users output a training metrics JSON or images (plots) and upload them to the repo (often under a "results" folder in the model repo). It's not automatic, but you can do it via` upload_file` in your training script. The Hub will display common formats (like images in README or JSON if integrated in model card).


10.


**Q:** Does huggingface_hub support repository branches and merging?
**A:** It supports branches: you can specify revision names to create branches on upload. It doesn't have an automatic merge feature in Python (no PR merges), but you could clone via` Repository` , merge with git commands, then push. For model repos, often merging isn't needed unless multiple people contribute, in which case they'd likely coordinate via the Hub web interface or a git workflow.


11.


**Q:** What is the Inference API and how do I call it with huggingface_hub?
**A:** The Inference API is a hosted service that allows you to get predictions from models on the Hub without loading them locally. In huggingface_hub, you use` InferenceClient` . For example,` client = InferenceClient(model="distilbert-base-uncased-finetuned-sst-2-english"); client.text_classification("I love this!")` returns the sentiment classification result.


12.


**Q:** Are there rate limits when using huggingface_hub to download models?
**A:** For downloads of public models, there aren't strict rate limits beyond extremely high volume (the Hub might throttle if you do something like thousands of requests per minute). For the Inference API or for certain endpoints like search, there are rate limits especially if not authenticated or on a free plan. Using your token (especially if you have upgraded plan) gives higher limits.


13.


**Q:** Can huggingface_hub interact with the Hugging Face Dataset Hub?
**A:** Yes, it works similarly for datasets. You can list datasets (` list_datasets` ), get dataset info (` dataset_info` ), download dataset files with` hf_hub_download` or` snapshot_download` by specifying` repo_type="dataset"` or just using the dataset ID (which usually has a namespace too). The logic for caching and file patterns is analogous.


14.


**Q:** How do I share a model privately with someone using huggingface_hub?
**A:** You can create a repo and set it to private on the Hub (via website or with` create_repo(..., private=True)` ). Then upload your model. To let someone else access it, add them (or their HF organization) as a collaborator on that repo. They can then use huggingface_hub with their token and the same functions to download it. Private models require authentication tokens to access.


15.


**Q:** What does` exist_ok=True` do in create_repo?
**A:** It tells the function not to raise an error if the repo already exists. So if you run create_repo on an existing repo and` exist_ok=True` , it will simply not duplicate or error – it's used to make scripts idempotent (so you can run them multiple times without failing if the repo is already there).


16.


**Q:** Can I list files from a private repo using huggingface_hub?
**A:** Yes, if you're authenticated with access. Use` HfApi().list_repo_files("user/private-repo", token=your_token)` . It will return the files list, given your token has read permission.


17.


**Q:** How to retrieve the model card content (README) through huggingface_hub?
**A:** It's just a file in the repo (README.md). You can fetch it like any other file:` hf_hub_download("user/model", "README.md")` and then read the returned file path. Additionally,` model_info` returns a` description` field which is the README content in HTML or markdown format.


18.


**Q:** Does huggingface_hub verify file integrity after download?
**A:** Yes, it uses the ETag (which is essentially a hash) to verify that the downloaded file matches the expected content. If the content was truncated or corrupted, the hash check would fail and likely it would retry. It's generally reliable in ensuring correct downloads.


19.


**Q:** What is the difference between` discussions` and` pull requests` on the Hub, and can huggingface_hub interact with those?
**A:** Discussions are like issues for a model (community can ask questions or report issues) – huggingface_hub does not directly handle them; they are done via the website. Pull requests (now called "Link requests" or "Model edits") allow contributions from others. huggingface_hub itself doesn’t have methods for interacting with PRs or discussions; those are more interactive and done via web or git manually. You could integrate via GitHub API if necessary, but huggingface_hub is mainly about file and model management.


20.


**Q:** How do I handle repository permissions (like who can push) with huggingface_hub?
**A:** The permissions are managed on the Hub website or via the CLI by adding collaborators or setting the repo under an organization. huggingface_hub will enforce them – if you attempt to push to a repo you don’t have write access to, it will throw an authorization error. There's no direct Python call to add a collaborator; that you do on the Hub UI or via an API endpoint not exposed in huggingface_hub.


21.


**Q:** Can huggingface_hub be used for CI/CD to automatically push new model versions?
**A:** Absolutely. Many teams do that. You'd use HfApi or functions within a CI script to login with a token (often stored as secret) and then call` upload_file` or` upload_folder` after training to push the model. It's script-friendly and can be part of your CI jobs.


22.


**Q:** What happens if two people push to the same repo at the same time via huggingface_hub?
**A:** The last push will win for the main branch, and the other might be behind. There's no locking mechanism beyond git's own commit handling. If they changed different files, both commits exist (history diverges) and Hub might show them one after the other in commit history but not merge automatically – you'd then have to resolve by merging branches or reapplying changes. In general, coordinate so that simultaneous writes are minimized, or use branches for separate work then merge.


23.


**Q:** How to retrieve a dataset from the Hub with huggingface_hub for use with pandas or PyTorch?
**A:** You can use` hf_hub_download` to get the raw files (like CSVs) of a dataset repo. For instance:` csv_path = hf_hub_download("dataset/user_dataset", "train.csv", repo_type="dataset")` . Then you can load that CSV with pandas as normal. Alternatively, the` datasets` library uses huggingface_hub internally, so one often just uses` datasets.load_dataset("user_dataset")` without manual calls.


24.


**Q:** Does huggingface_hub support downloading from a specific subfolder of a repo?
**A:** You provide the full path in` path_in_repo` . For example, if the repo has` subdir/model.bin` , you call` hf_hub_download("user/repo", "subdir/model.bin")` . The library will handle directories accordingly (creating local subfolders as needed).


25.


**Q:** How can I rename or transfer a repository via the API?
**A:** Currently, there’s no direct function to rename or transfer ownership via huggingface_hub Python. Those actions are done on the website (Settings tab of the repo). If you needed to do it programmatically, you'd have to call the Hub REST API endpoints directly (not wrapped in the Python SDK as of now).


26.


**Q:** Is it possible to clone a repository with Git credentials from huggingface_hub?
**A:** huggingface_hub's` Repository` can clone using your token (it sets up a git credential helper automatically when you use` Repository.use_auth_token=True` ). Alternatively, you can manually` git clone https://user:token@huggingface.co/username/repo.git` . The Python library aims to abstract that but you can always fall back to raw git if needed.


27.


**Q:** Can huggingface_hub handle binary files like images or audio?
**A:** Yes, it handles any file type. You can upload images, audio, pickle files, etc., just like any file. If viewing on the Hub, some common formats get special preview (like images display, audio can play). From code, they're all just bytes that huggingface_hub transfers.


28.


**Q:** How do I ensure my model card (README) is properly displayed and includes metadata tags?
**A:** Follow the model card guide: include a YAML section at the top with keys like` license` ,` language` ,` tags` ,` datasets` if relevant. For example:


```text
--- license: mit language: en tags: - text-classification - sentiment---
```


Then the rest in markdown. huggingface_hub will treat that YAML as the metadata (ModelInfo will reflect those fields too).


29.


**Q:** Does huggingface_hub allow me to get the commit hash of the latest revision?
**A:** When you download, the cache directory name often includes the commit hash. Also,` model_info("user/model")` returns an` sha` attribute which is the current commit hash on main. So yes, you can retrieve it via the model info.


30.


**Q:** How can I programmatically like or dislike a model or add a comment using huggingface_hub?
**A:** There is currently no function in huggingface_hub to like a model or post a comment. Those actions are intended to be user-driven via the website. The API might have endpoints, but they're not exposed for general use in the Python client.


### 4. Troubleshooting and errors


1.


**Q:** I'm getting` RepositoryNotFoundError` when trying to download – what's wrong?
**A:** This means the repo name you provided doesn’t exist or is typed incorrectly, or you don’t have access. Check the` repo_id` for typos (including capitalization). If it's a private repo, ensure you passed your token or are logged in.


2.


**Q:** I got a 401 Unauthorized error calling upload_file – how do I fix it?
**A:** A 401 indicates you're not authenticated or lack permission. Make sure you've logged in via` huggingface-cli login` or passed a valid token to the upload call. Also verify you have write access to the target repo (for example, correct username or org).


3.


**Q:** Why do I see` ValueError: The current git user is` ... when using Repository?
**A:** This happens if git is not configured with your name/email or if credentials aren't set. The Repository class tries to make a commit; ensure you've set` git config --global user.email` and` user.name` , or pass` huggingface_token` to Repository so it can handle auth.


4.


**Q:** My upload fails partway for a large file – how can I troubleshoot?
**A:** For very large files, ensure stable internet. If it fails consistently, try the CLI as an alternative or smaller chunking (the library should chunk automatically, though). You might also want to update huggingface_hub – improvements in large file handling have been added over time.


5.


**Q:** I'm seeing` hf_transfer` or` xet` errors in logs when uploading – what's that?
**A:** The Hub backend uses a system called Xet for efficient large file storage. The library might log something about it if using the hf-xet integration. If an error occurs there, it could be a transient internal issue – you could attempt the upload again, or contact HF support if persistent.


6.


**Q:** After pushing, the model card isn't updating or still shows old info – why?
**A:** The model card (README) might be cached by the browser; try refreshing without cache. Or you might have multiple branches – ensure you pushed to main (the Hub shows main by default). If you committed via git, double-check you pushed the correct branch.


7.


**Q:** I'm getting` OSError: Could not load token` when trying to login or use API – what does it mean?
**A:** This indicates the library attempted to find your token (from` ~/.huggingface/token` or env var) but failed. Re-run` huggingface-cli login` to ensure the token is saved, or explicitly pass your token string in the call.


8.


**Q:** My inference calls are timing out or give 503 errors frequently – how to handle?
**A:** The model might be too large and not loaded yet, or the free Inference API is limited. For critical uses, consider using a dedicated inference endpoint (requires HF Inference subscription or deploying your own server). In code, you can catch` InferenceTimeoutError` and implement a retry or fallback to local inference.


9.


**Q:** When I call` HfApi().list_models` I got a` TypeError: ModelInfo is not JSON serializable` – what's wrong?
**A:** Possibly you're trying to print or json-dump the ModelInfo list directly. ModelInfo is a dataclass-like object but not pure JSON. You might need to manually extract fields or use` asdict(model_info)` if you want a dictionary. If it's some other TypeError, maybe mismatched huggingface_hub version with how you're using it.


10.


**Q:** I get` requests.exceptions.SSLError` on any huggingface_hub operation – how to fix it?
**A:** This indicates an SSL certificate validation problem (often in corporate networks or outdated cert stores). Updating certifi or setting the` REQUESTS_CA_BUNDLE` environment var to your company’s CA cert file often resolves it. As a last resort, you can set` verify=False` via a session, but that’s not recommended.


11.


**Q:** The Hub shows my model file but Transformers can't load it, complaining about missing keys – did I upload incorrectly?
**A:** Possibly you only uploaded weights but not the config or tokenizer needed. Ensure you uploaded all necessary files (config.json, vocab files, etc.). If keys are missing, could be a mismatch in naming convention or incomplete push. Check that the repo structure on Hub matches what` from_pretrained` expects.


12.


**Q:** I tried to push a git repo with many files and it fails (413 Payload Too Large) – what can I do?
**A:** The huggingface_hub API has file size limits per request. If you have many files, use` upload_folder` (which will upload sequentially) or split into multiple smaller commits. Also make sure not to exceed repository total size limits (if extremely large).


13.


**Q:** I'm encountering` HfHubHTTPError: 403 Forbidden` when trying to download a model I just made public – why?
**A:** Possibly caching: your token or environment might still be treating it as private. Try logging out (remove token) or ensuring you have the right permissions. It could also be that the model is gated (some models like LLaMA require acceptance, in which case login and accept license on the website first).


14.


**Q:** Why do I see a warning about symlinks on Windows when downloading?
**A:** huggingface_hub uses symlinks for efficient cache management on Windows if dev mode is off, it warns. To resolve, enable Developer Mode in Windows settings or run Python as admin to allow symlink creation. Otherwise, it falls back to copying, which is fine but uses more disk.


15.


**Q:** My search queries via` search_models` return only a few results, missing some models I expect – what could be wrong?
**A:** The search might be limited by default (e.g., returns 10 by default). Set` limit=100` or so to get more. Also ensure your filter isn't too restrictive. Alternatively, the missing models might not have the tags/keywords you're searching; try adjusting query or filter.


16.


**Q:** The progress bars for multi-file download (snapshot_download) are not showing in my script – how can I enable them?
**A:** huggingface_hub uses` tqdm` for progress in some functions by default. In non-interactive environments, you might not see them. If needed, wrap in your own tqdm or check if you can set` tqdm_class` parameter in snapshot_download to force output to console.


17.


**Q:** I'm uploading a model but the Hub UI says "This model is stored with Git LFS" and shows pointer content – did I do something wrong?
**A:** No, that's normal for large files. It means the file is stored via LFS. The UI doesn't display the raw binary (makes sense), but users can still download it. If you see pointer text instead of actual file, it's fine. Your` from_pretrained` will handle it under the hood by downloading the real weights.


18.


**Q:** My model has unusual file names (like containing spaces or unicode) – could that cause errors?
**A:** It's best to avoid spaces or problematic characters in file names. While huggingface_hub can handle them (URL-encoding them in requests), it's safer to use alphanumeric, hyphen, underscore. If you have to, ensure you pass the exact string (with proper escaping if manual). If you hit an issue, renaming files is the solution.


19.


**Q:** I accidentally pushed the wrong file (like a huge scratch file). How can I remove it from history?
**A:** Removing from commit history is tricky; you might need to rewrite git history which isn't supported via API. The simpler approach: delete the file in a new commit (it will still exist in history but not visible in current). If it's sensitive and you need it gone entirely, you might need to contact HF support. Generally, be cautious pushing secrets or large junk – there's no "purge" API.


20.


**Q:** Why do I get a` json.decoder.JSONDecodeError` when using HfApi().search_models or model_info?
**A:** This can happen if the API response isn't valid JSON (maybe a network error returned HTML or something). Possibly your connection was interrupted or you got an error page. Check your internet and token. If behind a proxy, ensure the response isn't an HTML login form. Adding debug logs or try/except to see raw response can help.


## Resources


- huggingface_hub official documentation:


[https://huggingface.co/docs/huggingface_hub](https://huggingface.co/docs/huggingface_hub)


- huggingface_hub github repository:


[https://github.com/huggingface/huggingface_hub](https://github.com/huggingface/huggingface_hub)


- huggingface_hub on pypi:


[https://pypi.org/project/huggingface-hub/](https://pypi.org/project/huggingface-hub/)


- hugging face hub platform docs:


[https://huggingface.co/docs/hub/index](https://huggingface.co/docs/hub/index)


- inference api documentation:


[https://huggingface.co/docs/api-inference/index](https://huggingface.co/docs/api-inference/index)


- huggingface_hub cli guide:


[https://huggingface.co/docs/huggingface_hub/guides/cli](https://huggingface.co/docs/huggingface_hub/guides/cli)


- cache management guide:


[https://huggingface.co/docs/huggingface_hub/guides/manage-cache](https://huggingface.co/docs/huggingface_hub/guides/manage-cache)


- model card best practices:


[https://huggingface.co/docs/hub/model-cards](https://huggingface.co/docs/hub/model-cards)


- security and privacy overview:


[https://huggingface.co/docs/hub/security](https://huggingface.co/docs/hub/security)


- hugging face forum (discuss):


[https://discuss.huggingface.co/](https://discuss.huggingface.co/)


- stack overflow tag:


[https://stackoverflow.com/questions/tagged/huggingface-hub](https://stackoverflow.com/questions/tagged/huggingface-hub)


- hugging face course:


[https://huggingface.co/learn](https://huggingface.co/learn)


- hugging face blog:


[https://huggingface.co/blog](https://huggingface.co/blog)


- github discussions for huggingface_hub:


[https://github.com/huggingface/huggingface_hub/discussions](https://github.com/huggingface/huggingface_hub/discussions)


- releases and changelog:


[https://github.com/huggingface/huggingface_hub/releases](https://github.com/huggingface/huggingface_hub/releases)


1.


1.


Katerina Hynkova
