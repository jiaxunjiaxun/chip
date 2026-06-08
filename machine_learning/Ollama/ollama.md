# Ollama and Open WebUI

## 介绍

```shell script
mkdir path_to_project
cd path_to_project
python3 -m venv venv

# 激活虚拟环境
source venv/bin/activate

# 关闭虚拟环境
deactivate
```

```shell script
TMPDIR=/path/to/pip_cache/
pip install --cache-dir=/path/to/pip_cache/ -e

TMPDIR=./pip_cache/ pip install [module_name] --cache-dir=./pip_cache/

TMPDIR=./pip_cache/ pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install --upgrade --cache-dir=./pip_cache/
```

