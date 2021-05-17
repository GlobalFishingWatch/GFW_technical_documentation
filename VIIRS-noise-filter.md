# Example query

VIIRS contains many false detections, especially around South America. The example query linked below reduces the noise in South America. (but it may eliminate some prat of true detection as well as South Atlantic Anomaly)

[Example query](https://github.com/GlobalFishingWatch/viirs-noise-filter/blob/main/code/get_viirs_without_noise.sql)

The filter basically adopts QF1,2,3,10 for outside of South America, while adopting some portion of QF1,2,3,5,7,10 based on the value of `Rad_DNB` and `SHI`.

# Repository

The filter was developed in the repository [viirs-noise-filter](https://github.com/GlobalFishingWatch/viirs-noise-filter).

You can see the visualization results we used to develop that filter in the link below.

https://github.com/GlobalFishingWatch/viirs-noise-filter/blob/main/rendered_output/01_develop_viirs_noise_filter.md