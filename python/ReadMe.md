# Chip for code and design

## php

### Python update with pip

    pip install -U pip
    pip freeze --local | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U --allow-all-external
