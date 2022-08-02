# Setting up ai2thor version 2.1.0 on GCP server

These are meant to be some useful tips for setting up ai2thor version 2.1.0 on GCP machines.

### System configuration

The CUDA driver version seems to have some impact on things. The following configuration works.
* CUDA driver version: 450.51.05 (Install NVIDIA-Linux-x86_64-450.51.05.run)
* CUDA version: 11.0

### Xorg setup
* Regenerate `/etc/X11/xorg.conf` using the following command
  
  `sudo nvidia-xconfig -a --use-display-device=None --virtual=1280x1024`
* Comment out the lines that say `Option	“UseDisplayDevice” “None”`

  Note: Only do this for 8 of the ‘Screen’s. If I do this for more than 8 screens, then ai2thor crashes for some reason.

* An example xorg.conf is attached for reference

### Testing ai2thor

`sudo X: 0` (Start xserver)

`DISPLAY=:0 python`
```
import ai2thor.controller
controller = ai2thor.controller.Controller()
controller.start()
controller.reset('FloorPlan28')
controller.step(dict(action='Initialize', gridSize=0.25))
```

### Other comments
* NVIDIA driver could be the main issue. Try removing all nvidia drivers and installing from fresh (Eg. Try the specific version above)
* Sometimes ai2thor may hang at the controller Initialization. These set of packages were useful to get around this issue: https://github.com/valtsblukis/hlsm/blob/main/hlsm-alfred.yml
* Useful links
  * https://github.com/Unity-Technologies/ml-agents/blob/main/docs/Training-on-Amazon-Web-Service.md
  * https://medium.com/@etendue2013/how-to-run-ai2-thor-simulation-fast-with-google-cloud-platform-gcp-c9fcde213a4a

### Xorg installation
```
sudo apt-get update
sudo apt-get install -y xserver-xorg mesa-utils
sudo nvidia-xconfig -a --use-display-device=None --virtual=1280x1024
```
