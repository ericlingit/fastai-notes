1. - [x] Setup environment on crestle/aws/paperspace
1. - [ ] Get comfortable with Jupyter and all other tools
    * - [x] python
    * - [x] jupyter
    * - [x] pandas
    * - [ ] numpy
1. - [x] Play with week1 code and understand it
    * - [x] re-train with my own images
    * - [x] Try different learning rates, epochs while running code
1. - [ ] Feel free to explore week2 notebook

---

## Environment setup
See https://github.com/ericlingit/fastai-notes/tree/master/_setup

## Get comfortable with tools
- [kaggle learn](https://www.kaggle.com/learn/overview): covers python, jupyter, and pandas.
- numpy: [tutorial](https://github.com/jamesdietle/fastaipart1v2/blob/master/Tutorials/NumpyTutorial.ipynb)

## Re-train resnet with my own images
### Classify [pictures of airliners and fighter jets](https://www.dropbox.com/s/o8ek1idxpqm5vd1/jets.zip?dl=0) (61.7 MB)
#### data folder structure:
- jets
    - train
        - airliners: 51 jpg images of airliners produced by boeing, bombardier, and embraer.
        - fighter-jets: 51 jpg images of western fighter jets such as the f-18, f-35.
    - test
        - airliners: 39 jpg images of airliners produced by airbus, such as the a380, a320.
        - fighter-jets: 39 jpn images of russian fighter jets produced by sukhoi and mikoyan, such as the su-35, mig-29.
    - valid
        - airliners: 20 jpg images of airliners produced by tupolev, such as the tu-154.
        - fighter-jets: 20 jpg images of korean and taiwanese fighter jets, such as the kai t-50 and aidc ching-kuo.

#### data source
People on fastai forums recommended using [google_image_download](https://github.com/hardikvasa/google-images-download) to search and automatically download images.

`$ pip install google_images_download`

Usage example:

`$ googleimagesdownload -k boeing-747 -l 15 -f jpg`

Explanation:
- `-k`: the search keyword: `boeing-747`
- `-l`: limit the number of images to download. I want `15` images (if unspecified, default is 100).
- `-f`: image format to filter for. I want `jpg` files.

This will create a `download/boeing-747` folder in your home directory, and fetch the first 15 images of boeing-747 found on Google.

#### Send images to paperspace vm
On local machine:
- zip up the pictures before uploading to paperspace VM:
    ```
    $ zip -r ~/downloads/jets.zip ~/downloads/jets
    ```
- Send it over:
    ```
    $ scp ~/downloads/jets.zip paperspace@paperspace:~/data
    ```

On remote VM:
- Unzip the images to the data directory
    ```
    $ cd ~/data
    $ unzip jets.zip
    ```
- Check the result:
    ```
    $ ls -l ~/fastai/courses/dl1/data/jets 
    $ tree ~/fastai/courses/dl1/data/jets
    ```

#### Launch jupyter notebook and have fun!
```
$ cd ~/fastai/courses/dl1/
$ jupyter notebook --no-browser
```

Update the cell with `PATH = "../../../data/dogscats/"`. Change it to `PATH = "../../../data/jets/"`

Next, just keep hitting `Shift+Enter` to run the cells that train the model and show the results :)

#### Results
Training results:

![Imgur](https://i.imgur.com/IKTmBA1.png)

Most correct airliners
![Imgur](https://i.imgur.com/10ZCyWP.png)

Most correct fighter jets
![Imgur](https://i.imgur.com/qHnpQNT.png)

Most incorrect airliners
![Imgur](https://i.imgur.com/DvjQ8J5.png)

Most incorrect fighter jets
![Imgur](https://i.imgur.com/W6v1KGB.png)

Most uncertain predictions
![Imgur](https://i.imgur.com/AErdGgX.png)

#### Observations
It seems that the most-correct airliners are all side views in which the elongated fuselage is clearly visible. Views from the front or back are more likely to be incorrect.

For fighter jets, the most-correct ones all have a view slightly above or below the plane. ie, you get to see either the back or the belly. Images with a view from the side are more likely to be incorrect.

Most images taken of airliners are views from the side. There is a clear lack of front or rear-view photos for airliners, and a lack of (horizontal) side view for fighter jets in my data.

---
## Try different learning rates, epochs while running code
> "If you use a smaller batch size, you may want to decrease the learning rate by the same ratio." - [Jeremy](https://forums.fast.ai/t/wiki-lesson-1/9398/5)



## Feel free to explore week2 notebook