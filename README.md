# jekyll-gallery
A Jekyll pure CSS responsive photo gallery

[**Jekyll Gallery**](http://github.com/oliviergranoux/jekyll-gallery) uses [Jekyll](https://jekyllrb.com/) Static Site Generator (SSG) to generate the website.

## Table of content

- [Features](#features)
- [Get Started](#get-started)
- [Resizing images in batch](#resizing-images-in-batch)
- [Author](#author)
- [Licence](#licence)
- [Credits](#credits)

## Features

This pure CSS gallery use HTML5, media queries, CSS3, SCSS & mixin. It does not use any JavaScript.

_Notes:_ 
* Tested with IE11 and recent versions of Edge, Chrome & Firefox, but not with Safari.
* the thumbnail display was originally developped with Firefox, thus display differs slightly with other browsers. Indeed, Chrome and Edge do not interpret `width: auto;` unlike Firefox.
* as the display of an album and its images uses only CSS (without JavaScript), the solution has a limitation: all images of an album have to be dowloaded, including large format for full screen displaying. Thus the size of download can be dramatically increased.
* I copied the content of `albums.md` in `index.md` instead to use `include_relative`. Indeed it seems that GitHub Pages does not interpret correctly it if these files include both layout variables, even it works locally.


## Get Started

A demo is visible on http://oliviergranoux.github.io/jekyll-gallery

### Install

1. Install [Jekyll](https://jekyllrb.com/)
2. Clone the git repo: `git clone https://github.com/oliviergranoux/jekyll-gallery.git`
3. Change folder: `cd jekyll-gallery`
4. Run the website with `jekyll serve`
5. open http://localhost:4000 on your browser

The static website is generated on `_site` folder.

### Configure the gallery
There are 3 sections to modify to put your own content: 

1. Adding pages 

Each page is in the folder `albums` and has the same content, except two variables `title` and `folder` that you need to specify.

_Note:_ the filename of the page must be the same than the value of `folder`.

2. Adding images

Images are in `assets/images/albums`. Create a folder which name is the same than the previous value `folder` and put your images inside.

_Notes:_

* Images have to be in `*.jpg` format,
* it is preferable to remove any space in the filename and use ASCII characters,
* Albums need several versions of each image (depending of width) and the suffix of each filename has to reflect the width:

| width       | suffix       |
| ----------- | ------------ |
| 600 px      | `-600w`      |
| 1200 px     | `-1200w`     |

_Note:_ I have voluntarily limited the format of images for optimisation. Indeed the page of each album will download all its images, including format used to display in full screen. Thus, it can dramatically increase the size of download with large format (like 2400px).

See below how to [resize images](#resizing-images-in-batch).
. The original image is not needed for the site, leave it only if you want to keep the original format.

3. Adding album's configuration

The file `_data/albums.yml` has to be modified to display correctly each album.

_Notes:_
* indentation and empty lines are important otherwise Jekyll will not be able to build the site. Do not forget to check the Jekyll build log if it does not build.
* order of albums in configuration is refleted while displaying the list of albums.
* idem for the order of images of each album. The first image is also used as thumbnail in the list of albums.
* captions are not mandatory and it is preferable to have short captions. Link is possible only in captions of images, but not in caption of albums.
* filename of images is that of the original image, despite the website does not use the original image.

4. Other configurations

Some configurations of the website are set in `_config.yml`:
* `url-path` need to be set correctly otherwise relative paths will not work,
* `author`,
* `sitename` for the name of the website,
* `labels` for all labels used. Do not hesite to translate them for your need.

Colors can be redefined from `_sass/colors.scss`, where they are based by default on three main colors. They can be change as you wish.

## Resizing images in batch

Here a simple solution I found to resize images in batch.

1. First install imagemagick within Ubuntu. On Windows 10, it is possible to use Ubuntu via WSL.

```
sudo apt-get install imagemagick
```

2. After that, navigate to the folder of your images, create a temporary folder `new` and execute for example this command line:

```
for file in *.jpg; do convert $file -resize '600' -set filename:base "%[basename]" "new/%[filename:base]-600w.jpg"; done;
```

This command line will resize all `*.jpg` images of the current folder with a width of 600px while maintening the ratio of images. All new images will be saved in 'new' folder with the suffix '-600w' in the filename. For explanation of line, you can check on the [documentation of ImageMagick](http://www.imagemagick.org/script/command-line-processing.php#geometry).

_Note:_ for small images which need to be enlarged, you have to use the following command (just change `-resize` option). Do not use it with images that need to be reduced: I noticed that will not reduce the size of generated image.

```
for file in *.jpg; do convert $file -resize '600x<' -set filename:base "%[basename]" "new/%[filename:base]-600w.jpg"; done;
```

_Note:_ this command line work only for `*.jpg` files and not `*.JPG` files. Plese adapt this line for each size wanted.

## Author
[Olivier Granoux](http://olivier.granoux.com)

## License
Repository is licensed under the [MIT license](LICENSE).

## Credits

Some solutions used to do this project:
- https://favicon.io/favicon-generator/ to generate the favicon, 
- The pure CSS gallery [Perfundo](https://github.com/maoberlehner/perfundo) that I adapted (check the file `_sass/perfundo-fix.scss`),

Here a non-exhaustive list of websites used for inspiration:
- https://html5up.net/uploads/demos/parallelism/
- http://miromannino.github.io/Justified-Gallery/getting-started/

