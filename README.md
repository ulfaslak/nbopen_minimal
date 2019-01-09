# `nbopen` minimal

Lightweight and minimal bash script for launching notebooks from the command line.

#### Advantages:
* Simple to use
* Aggregates launched notebooks on single notebook server
* No software to install

#### Disadvantages:
* Server must be launched from root directory
* Server must be launched on port 8888 (you can change this in the code though)
* No guarantee that it works AS-IS on your machine

## Installation
Copy the following code into your `.alias` file and source it:

```zsh
function nbopen(){
    # For this function to work, in .jupyter/jupyter_notebook_config.py, set `c.NotebookApp.notebook_dir = '/'` so notebook always launches from root
    if [ $# -eq 0 ] ; then
        if lsof -Pi :8888 -sTCP:LISTEN -t > /dev/null ; then
            echo "A Jupyter Notebook is already open on port 8888"
        else
            jupyter notebook --port 8888
        fi
    else
        if [ "${1:0:1}" != "/" ] ; then
            address=$(pwd)/$1
        else
            address=$1
        fi
        if lsof -Pi :8888 -sTCP:LISTEN -t > /dev/null ; then
            open http://localhost:8888/tree"$address"
        else
            jupyter notebook "$address" --port 8888
        fi
    fi
}
```

If you don't maintain a `.alias` file, chances are have a `.profile` file which is sourced at boot. You can put the code there. Remember to source it (like `source .profile`).

## Example use
Create new notebook server is none exists:

`$ nbopen`

Open a notebook. If no notebook server exists, one is created:

`$ nbopen path/to/my/notebook.ipynb`


