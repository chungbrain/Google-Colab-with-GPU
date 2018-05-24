# Google-Colab-with-GPU

I simply recommend an open platform, Google Colaboratory (Colab), which is able to compute data processing with various machine-learning algorithms with GPU.
It is easy to use and fast to compute and even free. (but, you couldn't use Colab in every time and day. Only 12 hours you can access this constantly.)
If you have connected your personal google drive with Colab, you can have an identical user-experience comparing to doing on your local machine.
In order to mount your codes to Colab's directory, you have to conjunction with Google drive before by simple code.

* 1. connecting between Colab and Google drive:
```
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse

from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()

import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}
```
During this process you are going to face a couple of authentification processes by personal google id, please insert your verification codes.


* 2. create new directory on Colab:
```
!ls -ltr
!mkdir -p drive
!google-drive-ocamlfuse drive
%cd drive
```


* 3. Clone the code from Github:
```
!git clone https://github.com/"REPOSITORY_NAME".git
```


* 4. If you want to use pytorch on Colab please follow these code below:
```
# Default Code
!pip3 install torch torchvision

# Code from Colab
from os import path
from wheel.pep425tags import get_abbr_impl, get_impl_ver, get_abi_tag
platform = '{}{}-{}'.format(get_abbr_impl(), get_impl_ver(), get_abi_tag())

accelerator = 'cu80' if path.exists('/opt/bin/nvidia-smi') else 'cpu'

!pip install -q http://download.pytorch.org/whl/{accelerator}/torch-0.3.0.post4-{platform}-linux_x86_64.whl torchvision
import torch
torch.__version__
```

* 5. uploading/downloading local files:
```
from google.colab import files

uploaded = files.upload()

for fn in uploaded.keys():
  print('User uploaded file "{name}" with length {length} bytes'.format(
      name=fn, length=len(uploaded[fn])))
      
      
from google.colab import files

with open('filename.txt', 'wb') as f:
    f.write(local_variable_name)

files.download('filename.txt')      
```


Now, enjoy your field trip to machine learning.
