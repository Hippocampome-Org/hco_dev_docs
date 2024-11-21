Orientation Angle Options
=========================

The options that exist for reporting an orientation angle are dsecribed in the article.

## Opexebo-based detection

This method uses an approach included in Opexebo [https://github.com/kavli-ntnu/opexebo](https://github.com/kavli-ntnu/opexebo). The code from Opexebo in python for the orientation angle detection was implemented in Matlab to include this method in GridMet's options.

Notes about the implementation include that it was found that Matlab computes standard deviation differently than Python's Numpy function for that. The same function name, nanstd(), was used in both languages but it was consistently observed to produce different results.

Another note is that Opexebo uses a peak detection approach to finding grid fields that was replaced with centroid location coordinates in GridMet for this orientation measurement. Opexebo's peak-based method uses a "mask" processing approach to finding coordinates. This often results in an array of field coordinates with many y = 0 and x = 0 points. GridMet does not include a "mask" approach, and therefore excludes such points as input to the orientation measurement function. This affects the orientation angle measured, and therefore GridMet's approach is considered to be an adapted version of Opexebo's methods. Tests have been run to compare Opexebo's peak-based orientation angle measurements with GridMet's cluster-based methods using Opexebo's orientation finding methods. The results have been found to be similar across many cells.