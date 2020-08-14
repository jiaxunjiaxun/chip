# Chip for code and design

## pip

### Python update with shell

``` shell
pip install -U pip
pip freeze --local | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U --allow-all-external

pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U

pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U
```

### Python update with python

``` python
#!/user/bin/python3

import pkg_resources
from subprocess import call

packages = [dist.project_name for dist in pkg_resources.working_set]

for pkg in packages:
    # python2 (deprecated)
    call('sudo -H pip install --upgrade ' + pkg, shell=True)
    # python3
    call('sudo -H pip3 install --upgrade ' + pkg, shell=True)
```
