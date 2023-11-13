Image data annotation with Matplotlib, OpenCV, and Python

![Jupyter Badge](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=fff&style=for-the-badge)
![Google Colab Badge](https://img.shields.io/badge/Google%20Colab-F9AB00?logo=googlecolab&logoColor=fff&style=for-the-badge)
![Python Badge](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=fff&style=for-the-badge)
![GitHub Badge](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=fff&style=for-the-badge)

## Installation guide

- Code editor
  ```
  https://colab.google/
  ```
- Clone the code repository from github
  ```
  !git clone https://github.com/Taaanvir/Image_annotation.git
  ```
- Unzip necessary folder from the repository
  ```
  !unzip -qq /content/Image_annotation/main.zip
  ```
- Change the current directory
  ```
  %cd main
  ```
- Import the necessary packages
  ```
  from skimage.metrics import structural_similarity as compare_ssim
  import matplotlib.pyplot as plt
  import argparse
  import imutils
  import cv2
  ```
- Function to convert the image frame BGR to RGB color space and display it
  ```
  def plt_imshow(title, image):
  	image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
   	plt.imshow(image)
   	plt.title(title)
   	plt.grid(False)
   	plt.show()
  ```
- Compute image annotation from parsing code with arguments and values
  ```
  # construct the argument parse and parse the arguments
  # ap = argparse.ArgumentParser()
  # ap.add_argument("-f", "--first", required=True,
  # help="first input image")
  # ap.add_argument("-s", "--second", required=True,
  # help="second")
  # args = vars(ap.parse_args())
  
  args = {
	"first": "images/original.png",
	"second": "images/modified.png"
  ```
- Load these two input images and convert into grayscale copy
  ```
  imageA = cv2.imread(args["first"])
  imageB = cv2.imread(args["second"])
  
  grayA = cv2.cvtColor(imageA, cv2.COLOR_BGR2GRAY)
  grayB = cv2.cvtColor(imageB, cv2.COLOR_BGR2GRAY)
  ```
- Compute the SSIM between these two images
  ```
  (score, diff) = compare_ssim(grayA, grayB, full=True)
  diff = (diff * 255).astype("uint8")
  print("SSIM: {}".format(score))
  ```
- Threshold the image annotation followed by the below finding contours
  ```
  thresh = cv2.threshold(diff, 0, 255,
	cv2.THRESH_BINARY_INV | cv2.THRESH_OTSU)[1]
  cnts = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,
	cv2.CHAIN_APPROX_SIMPLE)
  cnts = imutils.grab_contours(cnts)
  ```
- Loop over the contours and compute the bounding box to draw on both input images to represent where these two images differ
  ```
  for c in cnts:
	(x, y, w, h) = cv2.boundingRect(c)
	cv2.rectangle(imageA, (x, y), (x + w, y + h), (0, 0, 255), 2)
	cv2.rectangle(imageB, (x, y), (x + w, y + h), (0, 0, 255), 2)
  ```
- Show the output
  ```
  plt_imshow("Original", imageA)
  plt_imshow("Modified", imageB)
  plt_imshow("Diff", diff)
  plt_imshow("Thresh", thresh)
  ```

## Expected output demo

- ![](https://terabox.com/s/1hAxkCwWpgREKwzaE3WX7gA)
- ![](https://terabox.com/s/1bzumnVDDs-VpxvXqYS0mpw)
- ![](https://terabox.com/s/1bzlo54aDDEcyCGlNDvz4jQ)
- ![](https://terabox.com/s/1vlVawz9akNppxLFAlLYBvA)

