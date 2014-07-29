Fortune Speak
=============

Fortune Speak is a Python sample app for Managed VMs that synthesize and display a random fortune sound everytime you load the page.

It extends a traditional Python App Engine application with new
functionalities that are unlocked by Managed VMs
- *Get more CPU and RAM* by running your module on Google Compute Engine VMs
    - set GCE machine type in `app.yaml` ([view sources](app.yaml.py#L10))
- *Escape the sandbox* by writing to files and launching subprocess
    - cache wave file to local disk ([view sources](main.py#L41))
    - launch the `fortune` executable ([view sources](main.py#L33))
- *Customize your runtime* by installing third party packages
    - install the `fortune` package with `apt-get` ([view sources](Dockerfile#L3))
    - install `pyttsx` and `flask` from [`requirements.txt`](requirements.txt) with `pip` ([view sources](Dockerfile#L5))
- *Call into native* Python C extensions
    - import and call `_speak` `pyttsx` driver ([view sources](synth.py#L1))

## Prerequisites

During this step you will
- create a new Managed VMs project
- provision your local development environments
- run the final application locally and deploy it to production

### Create your project

1. Go to [Google Developers Console](//cloud.google.com/console) and create a new project.
2. Enable billing
3. Open `https://preview.appengine.google.com/settings?&app_id=s~<project>`
4. Click `Setup Google APIs project for VM Runtime...`

### Setup Docker

1. Install boot2docker
    - [MacOSX](http://docs.docker.io/installation/mac/) instructions
    - [Windows](http://docs.docker.io/installation/windows/) instructions
    - [Linux](https://github.com/boot2docker/boot2docker-cli/releases) releases
2. Setup and start boot2docker

        boot2docker init
        boot2docker up

### Setup the Cloud SDK

1. Get and install the SDK preview release
2. Setup the Managed VMs components:
        gcloud components update appengine-managed-vms
        gcloud auth login
        gcloud set project <project>
        gcloud preview app setup-managed-vms

### Run the application locally

1. Get the application code

        git clone https://github.com/proppy/appengine-vm-fortunespeak-python
        cd appengine-vm-fortunespeak-python

2. Run the application locally

        gcloud preview app run .

3. After seeing a request to `/_ah/start` in the logs open [http://localhost:8080](http://localhost:8080)


### Deploy the application to your project
      
1. Build and deploy the application image

        gcloud preview app deploy .

2. After the command complete succesfully open `https://<project>.appspot.com`

## Hello World!

During this step you will:
- create a simple Python hello world application

0. First reset your working directory to `step0`

        git reset --hard step0

1. Create a `app.yaml` file w/ the default App Engine configuration
2. Create a `main.py` file using `webapp2` w/ a `RequestHandler` that print `hello: <Country the HTTP request is coming from>`
3. Run locally and deploy it to production.

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step1)
- Compare with your working directory

        git diff step1

- If stuck, reset your working directory

        git reset --hard step1

## Hello Managed VMs

During this step you will:
- enable Managed VMs in your application configuration
- select a bigger instance class

1. Modify `app.yaml` to add `vm: true` 
2. Modify `app.yaml` to select a bigger instance class
3. Run locally
4. Notice that a Dockerfile has been created in your application directory
5. Deploy to production

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step2)
- Compare with your working directory

        git diff step2

- If stuck, reset your working directory

        git reset --hard step2

## Escape the sandbox

During this step you will:
- perform system operation: write to the local filesystem

1. Modify `main.py` to log the greetings to a file called 'messages.txt'
2. Modify `main.py` to add a new `RequestHandler` that display the content of this file.
3. Run locally and deploy it to production

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step3)
- Compare with your working directory

        git diff step3

- If stuck, reset your working directory

        git reset --hard step3

## Customize the Runtime Environment

During this step you will:
- add native dependencies to your application with `apt-get`
- perform system operattion: launch an external process

1. Modify `Dockerfile` to `RUN apt-get install -y fortunes`
2. Modify `main.py` to launch `/usr/games/fortunes`  with the `subprocess` module in the main `RequestHandler`, capture the standard output and display it back in the response.
3. Run it locally and deploy to production

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step4)
- Compare with your working directory

        git diff step4

- If stuck, reset your working directory

        git reset --hard step4

## Manage Python dependencies

During this step you will:
- managed your application dependencies with `pip` and `requirements.txt`
- rewrite your web application handler using a modern python framework `Flask`

1. Create a `requirements.txt` with `Flask` listed as a dependency
2. Modify `Dockerfile` to `RUN pip install -r requirements.txt -t .`
3. Edit `main.py` to use `flask.route` instead of `webapp2.RequestHandler`
4. Run it locally and deploy to production

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step5)
- Compare with your working directory

        git diff step5

- If stuck, reset your working directory

        git reset --hard step5


## Use Python C extensions

During this step you will:
- add `pyttsx` C extensions as a dependency of your application
- call into native code to perform text to speech.

1. Modify `requirements.txt` to list `pyttsx` as a depdency
2. Copy the file `synth.py` from the master branch

        git checkout master synth.py

3. Call `synth.Say` from you main request handler and return the wavform data with the `audio-x/wav` content type
4. Run it locally and deploy to production

### Solution

- Review the [solution](https://github.com/proppy/appengine-vm-fortunespeak-python/commit/step6)
- Compare with your working directory

        git diff step6

- If stuck, reset your working directory

        git reset --hard step6
