# digits_docker_jupyter

# Step 1 : Interact with the DIGITS Docker Container using Jupyter


```
jupyter notebook --no-browser --port=8889
```
Save/copy the token presented.

<kbd>
  <img src="/d_4_docker_jupyter_token.png">
</kbd>


# Step 2 : Port Forward on Local System

```
ssh -i ~/.ssh/gcp_new -N -f -L localhost:8888:localhost:8889 __user__name__@__external__ip__address
```

Say 'yes' for ECDSA key fingerprint.

# Step 3 : Launch Jupyter Notebook on Local System

Open a web-browser and go to [localhost:8888](localhost:8888)


## Enter the token, and click 'Login'

<kbd>
  <img src="/d_5_docker_jupyter_token_login.png">
</kbd>

## You will enter the jupyter notebook dashboard.

<kbd>
  <img src="/d_6_jupyter_dashboard.png">
</kbd>

## Click the New dropdown menu and seleck Python 2 Notebook.

<kbd>
  <img src="/d_7_jupyter_new_notebook.png">
</kbd>

## Enter the following in the new notebook.

```
!curl localhost:5000/index.json
```

## The following response indicates that digits is running on 'localhost:5000'

<kbd>
  <img src="/d_8_jupyter_new_notebook.png">
</kbd>





