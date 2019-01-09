# `nbopen` minimal

Lightweight and minimal bash script for launching notebooks from the command line.

#### Advantages:
* Simple to use
* Aggregates launched notebooks on single notebook server
* No software to install

#### Disadvantages:
* Server directory is hard coded (default is `/`, change at will)
* Server port is hard coded (default is `8888`, change at will)
* No guarantee that it works AS-IS on your machine. I wrote it up on a OSX. You may have to make a few changes to get it working on Linux, such as exchanging `open` with `xdg-open`.

## "Installation"
Copy the following code into your `.alias` file and source it:

```zsh
function nbopen(){
    if [ $# -eq 0 ] ; then
        if lsof -Pi :8888 -sTCP:LISTEN -t > /dev/null ; then
            echo "A Jupyter Notebook is already open on port 8888"
        else
            jupyter notebook --notebook-dir / --port 8888
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
            jupyter notebook "$address" --notebook-dir / --port 8888
        fi
    fi
}
```

If you don't maintain a `.alias` file, chances are have a `.profile` file which is sourced at boot. You can put the code there. Remember to source it (like `source .profile`).

## Example use
Create new notebook server if none exists:

`$ nbopen`

Open a notebook. If no notebook server exists, one is created:

`$ nbopen path/to/my/notebook.ipynb`


