1. - [x] Setup environment on crestle/aws/paperspace
1. - [ ] Get comfortable with Jupyter and all other tools
    * - [x] python
    * - [x] jupyter
    * - [x] pandas
    * - [ ] numpy
1. - [ ] Run week1 code and understand it. Play with code to understand it
    * - [ ] re-train with my own images
1. - [ ] Try different learning rates, epochs while running code
1. - [ ] Feel free to explore week2 notebook

## Environment setup
See https://github.com/ericlingit/fastai-notes/tree/master/_setup

## Get comfortable with tools
[kaggle learn](https://www.kaggle.com/learn/overview)

## Re-train resnet with my own images
### Classify pictures airliners and fighter jets
#### data folder structure
- train
    - airliners
    - fighter-jets
- test
    - airliners
    - fighter-jets
- valid
    - airliners
    - fighter-jets
#### data source
Use [google_image_download](https://github.com/hardikvasa/google-images-download) to search and automatically download what you want

`$ pip install google_images_download`

Usage example:

`$ googleimagesdownload -k boeing-747 -l 15 -f jpg`

Explanation:
- `-k`: keywords. your search term
- `-l`: limit. number of img to download. default is 100
- `-f`: format. jpg, png, gif, ...

This will create a `download/boeing-747` folder in your home directory, and download the first 15 images found on google.

