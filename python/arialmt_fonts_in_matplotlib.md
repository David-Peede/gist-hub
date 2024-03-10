# ArialMT Fonts in  `matplotlib`

I always forget that Arial is not a default font in `matplotlib` and it takes a hot second to figure out everything that is going on under the hood, so here is an example of how to get the ArialMT font family into `matplotlib`!


### Step 1. Acquire the Font Family

`matplotlib` needs `.ttf` font files, which are not trivial to acquire. As a convience you can download the ArialMT font family from [here](https://github.com/David-Peede/gist-hub/blob/main/data/ArialMT.zip) by clicking on the download raw file icon, after which you will need to unzip it like so.

```bash
unzip ArialMT.zip
```

If everything worked correctly you should now have a `ArialMT` directory.


### Step 2. Find your  `matplotlib`  Font Directory

You will need to find the directory  where `matplotlib`  store its fonts, here is how I did it. __NOTE:__ Your output will look different from mine.

```python
import matplotlib
print(f'{matplotlib.get_data_path()}/fonts/ttf')
>>> /users/dpeede/anaconda/anc_der_intro_env/lib/python3.10/site-packages/matplotlib/mpl-data/fonts/ttf
```


### Step 3. Clear the  `matplotlib`  Cache

You need to clear the existing font cache stored by `matplotlib` , which can be done by executing the following command from your home directory.

```bash
rm -fr ~/.cache/matplotlib
```


### Step 4. Move the Font Family `.ttf`  Files to Your  `matplotlib`  Font Directory

Lastly, you will need to move the `.ttf` files to the directory you found in step 2. For example, this can be achieved by using the `mv` command to move all the `.tff` files like so.

```bash
mv ./ArialMT/*.ttf /users/dpeede/anaconda/anc_der_intro_env/lib/python3.10/site-packages/matplotlib/mpl-data/fonts/ttf
```

Then you are all set to enjoy using the ArialMT font family! __NOTE:__ If you are using Jupyter you will need to restart the kernel.