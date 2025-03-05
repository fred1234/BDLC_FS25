# JupyterLab

## Installing a New Theme

1. Go to the `Extension Manager` (jigsaw icon) and `enable` the feature.
2. Search for `jupyterlab-theme-solarized-dark` and click `install`.
3. Refresh the Window.
4. Activate your new theme under `Settings > Theme > JupyterLab Solarized Dark`

## Installing Git Extension

1. Go to the `Extension Manager` (jigsaw icon).
2. Search for `git` and try(!) to install the `jupyterlab-git` extension.
3. Install the jupyterlab-git server extension with `pip install --upgrade jupyterlab-git`.
4. Turns out, we need to restart the server:
5. Unfortunately, we don't have a `restart` options in the web-interface. However, we can at least stop the service with `File > Shut Down`.
6. As `hadoop` on your ssh session, check that the shutdown was successful with:

   ```bash
   # this command should only return one line, the grep itself. Otherwise `kill` the jupyterlab process.
   ps -ef | grep jupyter
   ```

7. Start the service again:

   ```bash
   nohup jupyter lab > error.log &
   ```

8. Back in the web-service, we should see the new `git` icon in the left menu bar and a `git` menu entry on top.
9. Let's `clone` the BLDC repository. Make sure that in the file browser you are in `/` (you should e.g. see the folder `hadoop` and the file `error.log`). Go to `Git > Clone a Repository` and paste the url `https://github.com/fred1234/BDLC_FS25` into the text field and hit `Clone`.
