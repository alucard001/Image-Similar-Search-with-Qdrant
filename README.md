# Image Similar Search (image search image) using Qdrant

- [Image Similar Search (image search image) using Qdrant](#image-similar-search-image-search-image-using-qdrant)
  - [Purpose](#purpose)
  - [Findings](#findings)
  - [My notes on this mini project](#my-notes-on-this-mini-project)

This mini project does the following:

- Turn images (`.jpg`) to vector using OpenCV
- Add the vector to Qdrant server
- Search image using Qdrant client
- Everything Python, no API
- But you can always turn it to REST API to use as an image search service

## Purpose

- To test how to use Qdrant as a vector database server to search image using image.
  - i.e. If I pass in an image, can it fetch similar images from our archive
- I use `qdrant_client.http.models.Distance.COSINE` as default way to find similar images.
- You can always use other algorithm like `DOT`, `EUCLID` and `MANHATTAN` to see the performance.

## Findings
- From my testing:
  - `COSINE` and `DOT` return reasonable result
  - `EUCLID` and `MANHATTAN` return no result.

## My notes on this mini project

- Code is duplicated on purpose.
- Qdrant Python API doc is difficult to read, I have to [go back to another example](https://colab.research.google.com/github/qdrant/examples/blob/master/qdrant_101_getting_started/getting_started.ipynb#scrollTo=5-Z4LbHE4lws) to see how things actually work.
  - Keywords: **Actually**
  - It lacks concrete, how-things-actually-work example
- Using `cv2.imread(image)` will give grayscale image, even if I confirm that I am passing the original image path to it.
  - No, `cv2.IMREAD_UNCHANGED` did not work
  - I read [this question](https://stackoverflow.com/questions/75952834/why-does-opencv-imread-return-grayscale-images-despite-color-flag) already, yet it still did not work.
- The original dataset has already had duplicated images.  The difference is the size (width and height) of the image.
- Can I pass multiple images to search similar images?
  - Yes, you use `SearchRequestBatch`.  I haven't tested it, but the program flow is the same.
  - You can also use `RecommendRequest` for this purpose, but I don't understand the doc at this moment.