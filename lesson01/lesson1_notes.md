## Why not run on Google Colab?
If you get disconnected after idling for a while, [you lose the session](https://forums.fast.ai/t/google-colab-setup-for-fastai-part-2-v2/13464/3). You have to reinstall all of the dependencies _again_. Yes, the setup can be automated with a script. I'm sure someone wrote it and shared it on github, but running the script and redownloading everything takes a lot of time. It's too much of a hassle. I'd rather focus on learning the lesson. If you're just taking lesson 1 out for a test-drive, Colab is fine. Going through the whole course on Colab? Nope.

---
## [Problem](https://i.imgur.com/OwmikTw.png) encountered on launching lesson1.ipynb:
```
Notebook failed to load
The error was: [sprintf] expecting number but found string
See the error console for details.
```
### Environment info:
- Paperspace fast.ai template
- Jupyter & python version:
  ```
  Server Information:
  You are using Jupyter notebook.

  The version of the notebook server is: 5.2.2
  The server is running on this version of Python:
  Python 3.6.4 |Anaconda, Inc.| (default, Dec 21 2017, 21:42:08) 
  [GCC 7.2.0]

  Current Kernel Information:
  Python 3.6.4 |Anaconda, Inc.| (default, Dec 21 2017, 21:42:08) 
  Type 'copyright', 'credits' or 'license' for more information
  IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.
  ```

### Fix the error:
1. Revert the lesson notebook (because the error will crop out half of the notebook and autosave):
    ```
    $ cd fastai
    $ git checkout -- courses/dl1/lesson1.ipynb
    ```
1. [Update](https://github.com/fastai/course-v3/issues/64#issuecomment-434501646) `notebook`, this should update to version >= 5.7.4
    ```
    $ conda update notebook
    Fetching package metadata ...........
    Solving package specifications: .

    Package plan for installation in environment /home/paperspace/anaconda3/envs/fastai:

    The following NEW packages will be INSTALLED:

        prometheus_client: 0.5.0-py36_0         
        send2trash:        1.5.0-py36_0         

    The following packages will be UPDATED:

        jupyter_client:    5.1.0-py36h614e9ea_0  --> 5.2.4-py36_0         
        notebook:          5.2.2-py36h40a37e6_0  --> 5.7.4-py36_0         
        pyzmq:             16.0.3-py36he2533c7_0 --> 17.0.0-py36h14c3975_0
        terminado:         0.6-py36ha25a19f_0    --> 0.8.1-py36_1         
        zeromq:            4.2.2-hbedb6e5_2      --> 4.2.3-h439df22_3     

    Proceed ([y]/n)? y

    zeromq-4.2.3-h 100% |################################| Time: 0:00:00  44.95 MB/s
    prometheus_cli 100% |################################| Time: 0:00:00  55.14 MB/s
    pyzmq-17.0.0-p 100% |################################| Time: 0:00:00  60.83 MB/s
    send2trash-1.5 100% |################################| Time: 0:00:00  41.49 MB/s
    terminado-0.8. 100% |################################| Time: 0:00:00  37.73 MB/s
    jupyter_client 100% |################################| Time: 0:00:00  68.73 MB/s
    notebook-5.7.4 100% |################################| Time: 0:00:00  66.95 MB/s
    ```
1. Important: update jupyter notebook config:
    ```
    $ jupyter notebook --generate-config ~/.jupyter/jupyter_notebook_config.py "c.NotebookApp.ip = '127.0.0.1'"
    ```
  - When prompted to overwrite, press y and enter.

If you skip the last step, you'll get [the following error](https://forums.fast.ai/t/jupyter-notebook-keyerror-allow-remote-access/24392/9) when launching jupyter notebook:
```
$ jupyter notebook --no-browser
Traceback (most recent call last):
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/traitlets/traitlets.py", line 528, in get
    value = obj._trait_values[self.name]
KeyError: 'allow_remote_access'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 864, in _default_allow_remote
    addr = ipaddress.ip_address(self.ip)
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/ipaddress.py", line 54, in ip_address
    address)
ValueError: '' does not appear to be an IPv4 or IPv6 address

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/paperspace/anaconda3/envs/fastai/bin/jupyter-notebook", line 11, in <module>
    sys.exit(main())
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/jupyter_core/application.py", line 266, in launch_instance
    return super(JupyterApp, cls).launch_instance(argv=argv, **kwargs)
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/traitlets/config/application.py", line 657, in launch_instance
    app.initialize(argv)
  File "<decorator-gen-7>", line 2, in initialize
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/traitlets/config/application.py", line 87, in catch_config_error
    return method(app, *args, **kwargs)
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 1628, in initialize
    self.init_webapp()
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 1378, in init_webapp
    self.jinja_environment_options,
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 159, in __init__
    default_url, settings_overrides, jinja_env_options)
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 252, in init_settings
    allow_remote_access=jupyter_app.allow_remote_access,
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/traitlets/traitlets.py", line 556, in __get__
    return self.get(obj, cls)
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/traitlets/traitlets.py", line 535, in get
    value = self._validate(obj, dynamic_default())
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/site-packages/notebook/notebookapp.py", line 867, in _default_allow_remote
    for info in socket.getaddrinfo(self.ip, self.port, 0, socket.SOCK_STREAM):
  File "/home/paperspace/anaconda3/envs/fastai/lib/python3.6/socket.py", line 745, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
socket.gaierror: [Errno -2] Name or service not known
```

---

## Discovered that there are 2 version of the course on github:
1. One located at https://github.com/fastai/fastai/tree/master/courses
1. Another one at https://github.com/fastai/courses

The first one appears more current, and is bundled with Paperspace's fastai template.

