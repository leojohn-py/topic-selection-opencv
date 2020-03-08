# topic-selection-opencv
# topic-selection-opencv
#output
#https://photos.app.goo.gl/YCteBdy75Ji1QLj57

import sys
import math
import cv2 as cv
import numpy as np

def main(argv):
    image = 'C:/Users/john1/Desktop/road.jpg'
    filename = argv[0] if len(argv) > 0 else image
    # Loads an image
    src = cv.imread(cv.samples.findFile(image), cv.IMREAD_GRAYSCALE)

    dst = cv.Canny(src, 50, 200, None, 3)

    # Copy edges to the images that will display the results in BGR
    cdst = cv.cvtColor(dst, cv.COLOR_GRAY2BGR)
    cdstP = np.copy(cdst)

    lines = cv.HoughLines(dst, 1, np.pi / 180, 150, None, 0, 0)

    if lines is not None:
        for i in range(0, len(lines)):
            rho = lines[i][0][0]
            theta = lines[i][0][1]
            a = math.cos(theta)
            b = math.sin(theta)
            x0 = a * rho
            y0 = b * rho
            pt1 = (int(x0 + 500 * (-b)), int(y0 + 500 * (a)))
            pt2 = (int(x0 - 500 * (-b)), int(y0 - 500 * (a)))
            cv.line(cdst, pt1, pt2, (0, 0, 255), 3, cv.LINE_AA)

    cv.moveWindow('Source', 420, 150)
    cv.imshow("Source", src)
    cv.imshow("Detected Lines (in red) - Standard Hough Line Transform", cdst)
    cv.waitKey(0)

if __name__ == "__main__":
    main(sys.argv[1:])
