1. - [x] Setup environment on crestle/aws/paperspace
1. - [ ] Get comfortable with Jupyter and all other tools
    * - [x] python
    * - [x] jupyter
    * - [x] pandas
    * - [ ] numpy
1. - [ ] Run week1 code and understand it. Play with code to understand it
    * - [x] re-train with my own images
    * - [ ] Try different learning rates, epochs while running code
1. - [ ] Feel free to explore week2 notebook

## Environment setup
See https://github.com/ericlingit/fastai-notes/tree/master/_setup

## Get comfortable with tools
[kaggle learn](https://www.kaggle.com/learn/overview)

## Re-train resnet with my own images
### Classify pictures of airliners and fighter jets
#### data folder structure: `/home/downloads/jets`
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
Use [google_image_download](https://github.com/hardikvasa/google-images-download) to search and automatically download what you want

`$ pip install google_images_download`

Usage example:

`$ googleimagesdownload -k boeing-747 -l 15 -f jpg`

Explanation:
- `-k`: keywords. your search term
- `-l`: limit. number of img to download. default is 100
- `-f`: format. jpg, png, gif, ...1

This will create a `download/boeing-747` folder in your home directory, and download the first 15 images found on google.

#### Send images to paperspace vm
I had download the images to my local machine so i can quickly examine them after they're downloaded.

To upload to paperspace vm, use `$ scp -r <source> <destination>`:
```$ scp -r ~/downloads/jets paperspace@paperspace:~/jets ```

The `-r` option is recursive. It'll send everything in that directory one-by-one over to your paperspace vm. If you have a lot of immages, this will be slow. It's better to zip up the pictures and send a single file instead:

```
$ zip ~/downloads/jets
$ scp ~/downloads/jets.zip paperspace@paperspace:~/
```
