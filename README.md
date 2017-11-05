# aws_setup
Various AWS scripts to make things a little less painful

Inspired by @radek’s blog post [here]{https://medium.com/@radekosmulski/automated-aws-spot-instance-provisioning-with-persisting-of-data-ce2b32bdc102} and his two forum posts ([here]{http://forums.fast.ai/t/from-zero-to-running-lesson-1-notebook-on-aws-instance-in-80-seconds/7184?u=wgpubs}, and [here]{http://forums.fast.ai/t/aws-gpu-install-script-and-public-ami/6990?u=wgpubs}), I present a generic way to get up and running on AWS in less than 60 seconds (well, 60 seconds after you go through it the first time :slight_smile: ).

## Prerequistes:

You must have created a persistent p2.xlarge instance in EC2 and give it a name. I simply followed the instructions from the introductory workshop material [here]{http://forums.fast.ai/t/wiki-lesson-1/7011} and named my instance “fastai-part1v2”. For storage, I went with 50GB.

You must have the AWC CLI installed and configured with your AWS Access Key, Secret Access Key, and default region.

## Steps:

* Clone my repo here: https://github.com/ohmeow/aws_setup4

* cd into the aws_setup directory

* In your terminal run. This will spin up your instance and ssh you into it.
```
$ bash start-aws-instance.sh {name-of-instance | defaults to fastai-part1v2}
```
* Once your ssh’d into your AWS instance, get and run my version of the install script (this step only needs to be ran once) :
```
$ wget https://raw.githubusercontent.com/ohmeow/aws_setup/master/fastai-install-gpu-part1-v2.sh
$ bash fastai-install-gpu-part1-v2.sh
```
* After that is done, start your jupyter notebook. This will activate the fastai anaconda environment that is created for you in the fastai-install-gpu-part1-v2.sh script and launch jupyter notebook.
```
$ ./start-jupyter-notebook
```
* In your browser, go to https://localhost:8888 and get to work

* When you are done, open up another terminal on your machine and run:  (This will shut down your EC2 instance)
```
$ bash stop-aws-instance.sh {name-of-instance | defaults to fastai-part1v2}
```
* Once you’ve gone through the steps above, you just have to …
```
$ bash start-aws-instance.sh {name-of-instance | defaults to fastai-part1v2}
$ ./start-jupyter-notebook
```
* Go to https://localhost:8888 and do your work
* when done.
```
$ bash stop-aws-instance.sh {name-of-instance | defaults to fastai-part1v2} 
```
## Notes:

* You’ll notice that while relying heavily on @radek’s version of the install script in step 4, I did make a few changes. The biggest one is simply using the environment.yml file in the fastai repo to create a “fastai” environment and install all the required packages for the course from it.

* bThis is for setting up a persistent EC2 instance. If you are into saving $$$ and want to see how to get things working with spot instances, see @radek’s posts mentioned above. I purposely decided to go with using persistent images (for the time-being at least) because it was much more straight-forward to get operational quickly. The scripts are more generic and will allow you to completely avoid having to log into AWS to be operational.

* This is a work in progress so please feel free to post recommendations and submit pull-requests if you got ideas on how to improve.

* I haven’t tested on anything outside of a p2.xlarge instance so if you are using other GPU friendly builds, let me know if the above still works or not.
